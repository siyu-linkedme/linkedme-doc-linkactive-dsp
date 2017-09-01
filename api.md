# 广告主对接文档
广告主向LinkActive平台提供沉默用户的设备ID可以通过两种方式，第一是离线方式，第二是在线方式。

请在接入前先了解这[两种方式及其优缺点](https://linkedme.gitbooks.io/linkactive-dsp/content/standard.html#312-沉默用户列表)，以便选择对您更优的方案。
以下是通过在线传输方式传输沉默用户的接口文档。

## 在线传输方式
在线传输沉默用户方式有两种，LinkActive提供接口和广告主提供接口。<font color="red">我们建议广告主提供接口来实时判断一个设备id是否是活跃(沉默)用户，以提高沉默用户判断的准确性</font>。


### 广告主需要的提供接口
#### 判断活跃（或沉默，活跃优先）用户提供接口（必须）
广告主提供，用于LinkActive平台调用，用来判断一个设备id是否为广告主的沉默用户（沉默用户是指在一定天数之内没有打开广告主App的用户）。
##### <font color="red">接口性能要求</font>：
满足从我们服务器上连接之后到数据传输结束时长在100ms以内（我们的机房在北京）

##### <font color="red">接口频次限制</font>：
如果广告主对接口频次有调用限制，每秒的请求数量放行在2000次以上。如果广告主能够提供批量接口，那么该数据可以相应降低（可以和技术同学商定）

##### 接口定义：
* URL：http://domain.com/ad/is_valid_id(广告主自己定义)
* Method：GET
* Description：用于LinkActive判断一个设备id是否为广告主的沉默用户id
* 参数说明（推荐，广告主自定义）

|参数|类型|是否必填|描述|
|--|--|--|--|
|id|String|必填|设备id|
|type|int|必填|设备id类型：<br>0：iOS<br>1：Android|
|is_md5|boolean|必填|设备id是原值(false)，还是md5值(true)|

* Response


```
{
"res":"true" 	//如果是活跃（沉默) 用户返回true，否则返回false
}
```

#### 用户拉活状态接口（非必须）
广告主提供，用户LinkActive平台调用，当平台接受到用户被激活状态的请求，调用该接口通知广告主。
接口定义：
* URL：http://domain.com/ad/active(广告主自己定义)
* Method：GET
* Description：用于LinkActive将激活状态同步给广告主
* 参数说明（推荐，广告主自定义）

|参数|类型|是否必填|描述|
|--|--|--|--|
|appid|String|必填|广告主的appid|
|type|int|必填|设备id类型：<br>0：iOS<br>1：Android|
|deviceId|String|必填|设备id|
|callback|String|必填|广告主检测到该用户激活，回调LinkActive的接口使用utf8进行urlencode|

* Response


```
{
"res":"true" 	//如果是沉默用户返回true，否则返回false
}
```

### LinkActive接口（非必须调用）
#### callback接口
当广告主发现该用户已经激活，那么调用该接口，通知LinkActive

* URL：在用户活跃状态接口中callback参数的值
* Method：GET
* Description：用于LinkActive记录设备激活状态



## 离线方式
广告主提供一个沉默用户的列表，LinkActive每次针对这个离线列表的数据进行沉默用户的匹配，相对在线传输方式效果较差。

## 关于打开app的scheme的说明
<font color="red">广告主需要准备一个打开app的scheme，并能统计这个scheme，以便在后期进行数据比对和结算</font>。scheme支持根据不同媒体的参数进行统计，例如: ws://index?openFrom={param}
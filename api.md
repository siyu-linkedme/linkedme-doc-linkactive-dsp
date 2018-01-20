# 广告主对接文档
以下文档主要介绍传输沉默用户的接口。

## 沉默用户传输方式
传输沉默用户方式有两种，LinkActive提供接口和广告主提供接口。<font color="red">我们建议广告主提供接口来实时判断一个设备id是否是沉默用户，以提高沉默用户判断的准确性</font>。


### 广告主需要提供的接口
#### 判断沉默/活跃用户接口（必须）
广告主提供，用于LinkActive平台调用，用来判断一个设备id(<font color="red">md5后的imei/idfa</font>)是否为广告主的沉默用户（沉默用户是指在一定天数之内没有打开广告主App的用户）。
##### <font color="red">接口性能要求</font>：
满足从我们服务器上连接之后到数据传输结束时长在100ms以内（我们的机房在北京）

##### <font color="red">接口频次限制</font>：
接口<font color="red">必须</font>支持批量请求，单次请求设备量为200，QPS≥1000。

##### 接口定义：
* URL：http://domain.com/ad/is_valid_id(广告主自己定义)
* Method：GET/POST
* Description：用于LinkActive判断多个设备id(<font color="red">md5后的imei/idfa</font>)是否为广告主的沉默用户id
* 参数说明（推荐，广告主自定义）

|参数|类型|是否必填|描述|
|--|--|--|--|
|ids|String|必填|md5后的设备id，Android使用md5(imei)，iOS使用md5(idfa)，使用逗号分隔|
|type|int|非必填|设备id类型：<br>0：iOS<br>1：Android，如果Android和iOS设备混在ids中，那么不传该参数|

* Response

如果是沉默用户接口，列表里返回<font color="red">沉默用户</font>的md5(id)；
如果是活跃用户接口，列表里返回<font color="red">非活跃用户</font>的md5(id)


```
{
"silent_ids":[id1,id2]	//如果是沉默用户接口，列表里返回沉默用户的md5(id)；如果是活跃用户接口，列表里返回非活跃用户的md5(id)
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

* Response


```
{
"res":"true" 	//如果是沉默用户返回true，否则返回false
}
```


## 关于打开app的scheme的说明
<font color="red">广告主需要准备一个打开app的scheme，并能统计这个scheme，以便在后期进行数据比对和结算</font>。scheme支持根据不同媒体的参数进行统计，例如: ws://index?openFrom={param}
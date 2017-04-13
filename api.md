# 沉默用户列表传输
广告主向LinkActive平台提供沉默用户的设备ID可以通过两种方式，第一是离线方式，第二是在线方式。

请在接入前先了解这[两种方式及其优缺点](https://linkedme.gitbooks.io/linkactive-dsp/content/standard.html#312-沉默用户列表)，以便选择对您更优的方案。
以下是通过在线传输方式传输沉默用户的接口文档。

## 在线传输方式
在线传输沉默用户方式有两种，LinkActive提供接口和广告主提供接口

### LinkActive提供接口
#### 沉默用户更新接口
LinkActive提供统一的沉默用户列表数据更新的接口，所有广告主调用此接口更新沉默用户的列表数据。

接口定义：
* URL：http://a.lkme.cc/ad/openapi/import_ids
* Method：GET
* Description：广告主在线向LinkActive平台导入沉默用户的设备id
* 参数说明

|参数|类型|是否必填|描述|
|--|--|--|--|
|idfa|String|必填|iOS系统设备唯一标识，可以填多个，用逗号分隔，最多100个|
|imei|String|必填|Android系统设备唯一标识，可以填多个，用逗号分隔，最多100个|
|android_id|String|可选|Android系统设备标识，可以填多个，用逗号分隔，最多100个|
|app_name|String|必填|广告主APP名称|
|linkedme_key|String|必填|LinkedME提供，广告主标识|
|ad_code|String|必填|LinkedME提供，广告拉活计划标识|
|is_md5|boolean|必填|广告主提供的idfa或者imei是原值为false，还是原值的MD5值为true|

* Response


```
{
"res":"ok" 
}
```


#### 自然活跃用户删除接口
广告主提供沉默用户列表后，这期间会有自然活跃的用户，需要广告主删除这部分用户，以免重复提活带来不必要的成本。
      
LinkActive平台提供了一个接口供广告主在线删除自然活跃用户设备id，接口定义如下：

* URL：http://a.lkme.cc/ad/openapi/delete_active_ids
* Method：GET
* Description：广告主从LinkActive平台在线删除自然活跃用户的设备id
* 参数说明

|参数|类型|是否必填|描述|
|--|--|--|--|
|idfa|String|必填|iOS系统设备唯一标识，可以填多个，用逗号分隔，最多100个|
|imei|String|必填|Android系统设备唯一标识，可以填多个，用逗号分隔，最多100个|
|android_id|String|可选|Android系统设备标识，可以填多个，用逗号分隔，最多100个|
|app_name|String|必填|广告主APP名称|
|linkedme_key|String|必填|LinkedME提供，广告主标识|
|is_md5|boolean|必填|广告主提供的idfa或者imei是原值为false，还是原值的MD5值为true|

* Response


```
{
"res":"ok" 
}
```


### 广告主提供接口
广告主提供接口，用于LinkActive平台调用，用来判断一个设备id是否为广告主的沉默用户。
接口定义：
* URL：http://domain.com/ad/is_valid_id(广告主自己定义)
* Method：GET
* Description：用于LinkActive判断一个设备id是否为广告主的沉默用户id
* 参数说明

|参数|类型|是否必填|描述|
|--|--|--|--|
|id|String|必填|设备id|
|type|int|必填|设备id类型：<br>0：iOS<br>1：Android|
|is_md5|boolean|必填|设备id是原值(false)，还是md5值(true)|

* Response


```
{
"res":"true" 	//如果是沉默用户返回true，否则返回false
}
```




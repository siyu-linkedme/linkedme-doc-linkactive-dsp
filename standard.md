# 需求说明

随着移动互联网由增量市场向存量市场的转化，移动APP的运营需求发生强烈的变化，开始由拉新向拉活转变。广告主面临着拉新用户越来越难，拉新流量成本越来越高，沉默用户也越来越多，沉默用户和卸载用户也不清楚等诸多问题。

由此，LinkActive拉活平台应运而生！LinkActive是LinkedME旗下专注于移动APP的“拉活平台”，帮助广告主解决沉默用户唤醒，提高活跃等诸多问题。

# 接入形式

广告主接入LinkActive平台不需要集成SDK，只需要向LinkedME提供沉默用户的设备ID列表以及APP的相关信息，LinkActive平台的投放运营人员负责素材的编辑及投放，广告主查看沉默用户的拉活效果。

# 接入流程
## 接入前提

1、<font color="red">广告主app必须能够支持deeplink调起</font><br>
2、广告主接入LinkActive拉活平台，需要提供两个方面的内容，第一是广告主APP相关信息；第二是沉默用户的列表；

### APP基本信息
广告主接入LinkActive拉活平台后，LinkActive平台的投放运营人员负责广告主的相关内容在媒体上进行投放。从而，广告主向LinkActive的运营人员提供APP的基本信息，以及投放的素材，比如Logo、文案、图片等等。具体如下所示：

|信息|说明|示例|
|--|--|--|
|ios_uri_scheme|1. iOS系统通过此值唤起APP；<br> 2. 此URI Scheme老版本的APP要能够识别；<br> 3. 该值需包含统计信息，用于标识来自LinkActive平台的拉活；<br> 4. 此URI Scheme要能够动态添加参数，便于跳转到APP的不同详情页面。<font color="red">(在媒体方投放的Banner，用户点击后，能够直接跳转到广告主APP的具体详情页面）</font> |sdk123://home/news?<br>gd_ext_json={"event":"home",<br>"label":"click_schema_linkactive"}|
|appstore_url|AppStore下载地址||
|android_uri_scheme|1. Android系统通过此值唤起APP；<br> 2. 此URI Scheme老版本的APP要能够识别；<br> 3. 该值需包含统计信息，用于标识来自LinkActive平台的拉活；<br> 4. 此URI Scheme要能够动态添加参数，便于跳转到APP的不同页面。<font color="red">(在媒体方投放的Banner，用户点击后，能够直接跳转到广告主APP的具体详情页面）</font> |sdk124://home/news?<br>gd_ext_json={"event":"home",<br>"label":"click_schema_linkedme“}|
|pkg_name|安卓包名||
|apk_url|Android下载地址||
|h5_url|和APP内的内容一致的h5页面链接||
|logo素材|各种尺寸的APP logo图片||
|文字及图片素材|在媒体上投放的素材内容|&nbsp;|


### 沉默用户列表
**沉默用户**，通常是指一段时间内没有打开APP的用户，而对于用户无打开行为时长多久才称为沉默，可以根据APP的性质来调整。例如新闻资讯类、直播类的APP，用户超过7天没使用的可以定义为沉默用户；社交类的APP，用户超过14天没使用的定义为沉默用户。  
我们通常建议连续<font color="red">3~7天未活跃</font>的用户即可定义为沉默用户，此时拉活沉默用户的效果最好。

广告主向LinkActive平台提供沉默用户设备ID的MD5数据即可，媒体方向LinkActive平台请求拉活广告资源时，也是以设备ID的MD5作为Key向LinkActive平台查询。因此对于LinkActive平台不知道广告主的具体沉默用户列表数据。传输方法具体参见[传输沉默用户列表API文档](/api.md)


|平台|设备ID|设备ID的MD5|示例|
|--|--|--|--|
|Android|imei|md5（imei）|IMEI：862033033085604<br>MD5（IMEI）：b0131ecfa9495650f1fee8e11dd7164f|
|iOS|IDFA|md5（IDFA）|IDFA：FFFF50E1-4F95-44E8-9D19-25FE4E5AB8F8 <br>md5（IDFA）：7ed40e55b2f37c519fa23a63d31d6cdc|

<font color="red">注意事项：imei和IDFA必须是原值，不要做大小写转化；</font>









## 接入流程

广告主接入LinkActive平台主要工作有两部分，第一部分是提供沉默用户列表数据；第二部分是提供APP相关信息，包括APP基本信息和素材内容。然后查看沉默用户的拉活即可。

LinkActive的工作原理：

![](/assets/ggz-new.jpg)


# 核对数据

LinkActive平台帮助广告主拉活沉默用户，广告主APP的后台服务器完全可以接口到各种沉默用户唤醒的访问请求。媒体方APP唤醒广告主APP时，媒体方会实时通知LinkActive平台，从而每一次沉默用户唤醒，<u>媒体方，广告主，LinkActive平台三方的拉活数据保持着一致性</u>。广告主和LinkActive平台核对数据时，把沉默用户唤醒的数据同步到平台。返回方式分为两种，第一种是实时同步；第二种是定期同步。
## 实时同步
当每一个沉默用户唤醒时，广告主实时向LinkActive平台同步该沉默用户被唤醒的数据。最终每个月（每一个月月初的第一个工作日）LinkActive平台归纳总结沉默用户唤醒的数据，并且和媒体方拉活广告主APP的数据进行比对，然后以报表的形式提供给广告主，做为最终结算的数据依据。如果核对数据过程中出现问题，双方及时协调解决。
## 定期同步
每天唤醒的沉默用户，广告主不用实时同步到LinkActive平台，在每个月（每一个月月初的第一个工作日）核对数据时，把已经唤醒的沉默用户列表数据告诉LinkActive平台，然后由LinkActive平台和媒体方比对数据，然后以报表的形式提供给广告主，做为最终结算的数据依据。如果核对数据过程中出现问题，双方及时协调解决。

# 结算费用

沉默用户唤醒费用的结算依据是数据；结算的时间，金额等等问题完全按照合同履行即可。



#疑问解答

Q1：如果沉默用户被LinkActive平台唤醒，在此期间该沉默用户被其他渠道也唤醒，比如用户从应用商店，该沉默用户被唤醒是否属于LinkActive平台的贡献？

A1：凡是被LinkActive平台唤醒过的沉默用户，都应该算是LinkActive平台的拉活用户。因为LinkActive平台的每一次有效唤醒都要给媒体方付费用，即LinkActive平台运作的主要成本。
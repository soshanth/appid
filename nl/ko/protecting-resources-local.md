---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-02"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}



#  {{site.data.keyword.appid_short_notm}}와 함께 작업할 수 있도록 로컬 개발 서버 구성
{: #protecting-local}

{{site.data.keyword.appid_short}} 서비스를 사용하도록 로컬 환경을 구성할 수 있습니다. 특히, {{site.data.keyword.appid_short_notm}} 서버 SDK를 사용하여 로컬로 코드를 개발함으로써 요청을 개발 서버에 전송할 수 있습니다.
{:shortdesc}


## 시작하기 전에
{: #begin}

[서버 SDK 설치](/docs/services/appid/install.html#nodejs-setup)를 완료했는지 확인하십시오.


## 로컬 개발 서버에 대해 작업할 수 있도록 {{site.data.keyword.appid_short_notm}} 애플리케이션 구성
{: #configuring-local}

로컬 개발 서버와 작업하도록 앱을 구성하려면 각 요청에서 로컬 호스트를 사용하십시오.

1. 테넌트 ID를 사용자의 {{site.data.keyword.appid_short_notm}} 테넌트 ID로 바꾸십시오. 서비스 대시보드에서 이 ID를 찾을 수 있습니다.
2. 다음 표에 표시된 대로 지역을 적합한 지역으로 바꾸십시오.

<table> <caption> 표 1. {{site.data.keyword.Bluemix_notm}} 지역 및 해당하는 Android 및 iOS용 {{site.data.keyword.appid_short_notm}} 지역 </caption>
<tr>
  <th> Bluemix 지역 </th>
  <th> Android 및 iOS </th>
</tr>
<tr>
  <td> 미국 남부 </td>
  <td> AppID.REGION_US_SOUTH </td>
</tr>
<tr>
  <td> 시드니 </td>
  <td> AppID.REGION_SYDNEY </td>
</tr>
<tr>
  <td> 영국 </td>
  <td> AppID.REGION_UK </td>
</tr>
</table>



### Android
{: #android}
```java
String baseRequestUrl = "http://localhost:<port>"; //set to your server running port
String tenantId = "your-AppID-service-tenantID";
String region = AppID.REGION_UK; //set your App ID application region here. Currently possible values are AppID.REGION_US_SOUTH, AppID.REGION_SYDNEY, or AppID.REGION_UK.

BMSClient bmsClient= BMSClient.getInstance();
bmsClient.initialize(getApplicationContext(), region);
AppID appId = AppID.getInstance();
appId.initialize(getApplicationContext(), tenantId, region);

AppIDAuthorizationManager appIDAuthorizationManager = new AppIDAuthorizationManager(appId);
bmsClient.setAuthorizationManager(appIDAuthorizationManager);

Request request = new Request(baseRequestUrl + "/resource/path", Request.GET);
request.send(this, new ResponseListener() {
    @Override
		public void onSuccess (Response response) {
        Log.d("Myapp", "onSuccess :: " + response.getResponseText());
		}
    @Override
		public void onFailure (Response response, Throwable t, JSONObject extendedInfo) {
        if (null != t) {
            Log.d("Myapp", "onFailure :: " + t.getMessage());
			} else if (null != extendedInfo) {
            Log.d("Myapp", "onFailure :: " + extendedInfo.toString());
		} else {
            Log.d("Myapp", "onFailure :: " + response.getResponseText());
		}
    }
});
```
{: codeblock}

### iOS - Swift
{: #swift}
```swift

let baseRequestUrl = "http://localhost:<port>"; //set to your server running port
 let tenantId = "your-AppID-service-tenantID"
 let region = AppID.REGION_UK; //set your App ID application region here. Currently possible values are AppID.REGION_US_SOUTH, AppID.REGION_SYDNEY, or AppID.REGION_UK.

BMSClient.sharedInstance.initialize(bluemixRegion: region)
BMSClient.sharedInstance.authorizationManager = AppIDAuthorizationManager(appid:AppID.sharedInstance)

var request:Request =  Request(url: baseRequestUrl + "/resource/path", method: HttpMethod.GET)
request.send(completionHandler: {(response:Response?, error:Error?) in
    if let error = error {
            print("Connection failure")
            print("Error :: \(error)");
            print("Status :: \(response?.statusCode)");
        } else {
            print("Connection success")
            print("Response :: \(response?.responseText)")
        }
});
```
{: codeblock}

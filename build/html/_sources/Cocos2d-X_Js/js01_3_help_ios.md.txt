
## iOS

이 액세스 문서는 cocos2d-x 3.16을 기반으로 합니다. 다른 버전의 cocos2d-x를 사용할 때 문제가 발생한다면 <br />
 UPLTV의 기술 팀에 문의하여 문제 해결 및 지원 서비스를 받을 수도 있습니다.

### 1. SDK 패키지 다운로드하기
먼저 [UPSDK 다운로드](http://doc.upltv.com/en/master/chapters/chapter09.html "SDKDownLoad")에서 UPSDK JsPlugin 패키지 다운로드하고 압축해제 하시면, <br />
아래와 같은 세 개의 디렉토리들이 보입니다.

- `UPSDK.framework`: UPSDK 메인 패키지이므로 현재 프로젝트에 추가합니다.
- `UPSDK.bundle`: UPSDK 메인 패키지가 현재 프로젝트에 액세스해야 하므로 외부 파일 리소스를 추가합니다.
- `UpltvjsBridge`: 이 디렉토리에는 현재 cocos2d-x js 프로젝트와 UPSDK AD 인터페이스 호출을 연동하기 위한 <br />
`*.js` 원본 파일들이 들어 있습니다.

</br>


### 2 UPSDK 메인 패키지 및 소스 추가하기
Xcode 프로젝트 디렉토리에 UPSDK. Framework, UPSDK.Bundle 및 UpltvjsBridge 폴더를 동시에 추가합니다. <br />
UpltvjsBridge 폴더에 `UpltvjsBridge` 및 `UpltvIos`  *.h 및 *.js 파일이 있는지 확인하시기 바랍니다. <br />
UpltvJsBridge 폴더에는 Object-C++ 소스코드와 *.js 스크립트 파일이 아래와 같이 있습니다.

UPSDK에 포함된 전체 파일은 다음과 같습니다.

![](http://docc.upltv.com/uploads/201805/5b02724dc9bb5_5b02724d.png)

`UPLTV.js` 와 `UPLTVIos.js` 를 프로젝트에 추가합니다.
권장 디렉토리`src -> upltv`는 아래와 같습니다.

![](http://docc.upltv.com/uploads/201805/5b02734555c95_5b027345.png)

`UPAdsBrigeJs.h` 와 `UPAdsBrigeJs.mm`를 현재 프로젝트에 추가합니다.
권장 디렉토리 `ios->upltv` 는 아래와 같습니다.

![](http://docc.upltv.com/uploads/201805/5b02736e0f931_5b02736e.png)

### 3. 광고 네트워크사 의존성 라이브러리 추가하기
UPSDK는 실행될 때 광고 네트워크사에 의존하므로 수동으로 의존 라이브러리 파일을 프로젝트로 가져와야 합니다. <br />
[여기](http://doc.upltv.com/en/master/chapters/chapter09.html "SDK第三方包下载")를 클릭하셔서 광고 네트워크사 의존성 패키지를 추가하시기 바랍니다.

UPSDK는 현재 다음의 광고 네트워크사에 의존합니다.

![Add all third party SDK packages](http://docc.upltv.com/uploads/201709/59afafb9143e9_59afafb9.png)

> 각 광고 네트워크사는 각각의 폴더에 해당하며, 폴더의 이름은 광고 네트워크사명의 접두사와 버전 번호로 <br />
표기하였기에 쉽게 식별할 수 있습니다. 폴더는 다른 의존 라이브러리 또는 원본 파일 외에 지원 리소스 파일을 <br />
포함할 수 있으므로 의존 라이브러리를 해당 지원 리소스와 함께 추가해야 합니다.


현재 프로젝트에 모든 광고 네트워크사를 추가할 필요는 없지만 더 많은 수익을 위해 프로젝트에 가능한 한 많은 <br />
광고 네트워크사 Ad 의존성 라이브러리 및 그에 수반되는 리소스 패키지를 추가하시기를 권장합니다. <br />
담당자에게 문의하여 추가 지원을 받아보세요.




광고 네트워크사를 추가한 후, XCode 프로젝트 `Linked Frameworks Libraries`에서 해당 라이브러리 파일을 <br />
올바르게 가져왔는지 확인합니다.


예를 들어, 현재 프로젝트 액세스에서 Applovin, Unity, Vungle, tapjoy 4개의 광고 라이브러리 <br />
(`AppLovinSDK.framework`, `UnityAds.framework`, `VungleSDK.framework`, `Tapjoy.framework`)를 추가합니다. <br />
성공적으로 추가한 후의 화면은 아래와 같습니다.

![](http://docc.upltv.com/uploads/201804/5acc6644c33a5_5acc6644.png)

위의 예시에서 Tapjoy AD는 일부 외부 리소스 파일에 액세스해야 하므로 프로젝트에 지원 리소스 파일을 <br />
추가해야 합니다. 지원 리소스 파일은 `TARGETS` –> `Build Phases` –> `Copy Bundle Resources`에 있습니다.

<br>
![](http://docc.upltv.com/uploads/201804/5acc70803fec8_5acc7080.png)


### 4. 시스템 의존성 추가하기.
의존성 라이브러리를 TARGETS → General → Linked Frameworks Libraries에 추가합니다.
- `QuartzCore.framework`
- `MediaPlayer.framework`
- `libsqlite3.tbd`
- `libz.tbd`
- `CoreMedia.framework`
- `CoreGraphics.framework`
- `CFNetwork.framework`
- `WebKit.framework` (선택사항)
- `WatchConnectivity.framework`	(선택사항)
- `SystemConfiguration.framework`
- `StoreKit.framework`
- `Social.framework`
- `MessageUI.framework`
- `JavaScriptCore.framework`	(선택사항)
- `EventKit.framework`
- `CoreTelephony.framework`
- `AVFoundation.framework`
- `AudioToolbox.framework`
- `AdSupport.framework`
- `GLKit.framework`
- `CoreMotion.framework`
- `libxml2.tbd`
- `libc++.tbd`
- `SafariServices.framework`
- `CoreLocation.framework`
- `EventKitUI.framework`
- `MobileCoreServices.framework`
- `GameController.framework`
<br>

### 프로젝트 구성
#### 1. 분류 컴파일러 추가하기

아래와 같이 `TARGETS` → `Build Setting` → `Linking` → `Other Linker Flags`에 `-ObjC`를 추가합니다.
![](http://docc.upltv.com/uploads/201804/5ae28f14f217a_5ae28f14.png)

#### 2. HTTP 모드와 호환을 위해 다음 노드를 info.plist에 추가하기

```objective-c
 <key>NSAppTransportSecurity </key>
 <dict>
	 <key>NSAllowsArbitraryLoads </key>
	 <true/>
 </dict>
```

#### 3. 권한을 얻기위해 다음 노드를 info.plist에 추가하기

(AdColony를 사용하는 경우, 추가해야 합니다, AdColony를 사용하지 않는 경우에는 추가하지 않아도 됩니다.).

```objective-c
 <key>NSCalendarsUsageDescription </key>
 <string>Some ad content may create a calendar event. </string>
 <key>NSCameraUsageDescription </key>
 <string>Some ad content may access camera to take picture. </string>
 <key>NSPhotoLibraryUsageDescription </key>
 <string>Some ad content may require access to the photo library. </string>
```

> 참고: 언어 및 사용 시나리오에 따라 개발자는 액세스 권한에 대한 설명을 적절하게 조정할 수 있습니다.
<br>

#### 4. Bitcode
Domob은 Bitcode를 지원하지 않습니다. 하지만 UPLTV에서는 Bitcode도 지원해드리고 있으니 필요하시면 <br />
담당자에게 문의하여 추가 지원을 받아보세요.

#### 5. Cocos 프로젝트 구성
Cocos를 이용하여 프로젝트를 빌드 하셨다면, `Version` 부분에 아래와 같이 입력하시기 바랍니다. <br />
(Cocos로 프로젝트를 빌드를 하시면 Version의 초기값은 수동으로 입력해야 합니다. 만약 버전 값을 입력하지 않으면 <br />
이후 사용에 영향을 초래할 수 있습니다.)

![cocos project Version field configuration](http://docc.upltv.com/uploads/201709/59afb01ec7612_59afb01e.png "cocos项目Version字段配置")
<br>

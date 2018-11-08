## CocoaPods Access Document
CocoaPods을 이용하신다면 아래와 같은 순서로 UPSDK를 가져오기 하시기 바랍니다. <br />
처음으로 CocoaPods을 이용하여 UPSDK를 연동하시는 분은 먼저, Avidly Repo를 CocoaPods에 추가합니다.
```
pod repo add Avidly https://github.com/guojunliu/AvidlyAdsSDK-SpecsRepo.git
```


기존의 UPSDK를 업데이트 하시려면 아래와 같은 코드를 사용하시기 바랍니다.

```
pod repo update Avidly
```

그리고 참조를 프로젝트 내의 `Podfile` 파일에 아래와 같이 추가합니다.

```
platform :ios, '8.0'
use_frameworks!

target ‘YourAppName’ do
  source 'https://github.com/guojunliu/AvidlyAdsSDK-SpecsRepo.git'
  pod 'UPSDK', '~> 3.0.03'
end
```

`~>  3.0.03`  표시는 버전을 나타내는 것이므로 최신 버전을 사용하시길 바랍니다.

프로젝트에 들어가서 아래와 같이 `pod install` 를 실행하시면 됩니다.

```
pod install
```

완료.


### 프로젝트 구성

#### 1 `info.plist`에 아래와 같이 노드를 추가하여 http 프로토콜을 승인합니다.

```objective-c
 <key>NSAppTransportSecurity </key>
 <dict>
	 <key>NSAllowsArbitraryLoads </key>
	 <true/>
 </dict>
```

#### 2 `info.plist`에 아래와 같이 노드를 추가하여 권한을 승인합니다.
```objective-c
 <key>NSCalendarsUsageDescription </key>
 <string>Some ad content may create a calendar event. </string>
 <key>NSCameraUsageDescription </key>
 <string>Some ad content may access camera to take picture. </string>
 <key>NSPhotoLibraryUsageDescription </key>
 <string>Some ad content may require access to the photo library. </string>
```


참고: 디스크립션 설정 언어를 변경하실수 있습니다.
<br>

#### 3 Bitcode
Youlan 과 Domob SDK는 Bitcode를 지원하지 않습니다. <br />
하지만 UPLTV에서는 Bitcode도 지원해드리고 있으니 필요하시면 담당자에게 문의하여 추가 지원을 받아보세요.
<br>

### Version
SDK 버전을 `UPSDKVersion.h`  파일에서 현재 SDK 버전 번호를 바로 확인 하실 수 있습니다.

```objective-c
//sdk version
#define UPSDKVERSION  @"3003"
```
> UPSDK의 현재 버전은 3003 입니다.

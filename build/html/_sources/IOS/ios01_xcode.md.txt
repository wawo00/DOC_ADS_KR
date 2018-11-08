## Xcode 액세스 문서
### SDK 다운로드
최신 SDK 폴더를 [이곳](https://upsdk-korean.readthedocs.io/ko/master/chapters/chapter09.html)에서 다운로드 하시고 압축을 해제하시면 아래와 같은 파일을 보실 수 있습니다.
- UPSDK.framework
- UPSDK.bundle

`UPSDK.framework` 는 SDK의 정적 라이브러리입니다. <br />
`UPSDK.bundle` 에는 정적 라이브에서 참조된 외부 리소스(이미지 등)가 포함되어 있습니다.
</br>

### Xcode프로젝에 SDK 추가하기
아래의 예시와 같이 `UPSDK.framework` 과 `UPSDK.bundle`를 Xcode 프로젝트에 추가합니다. <br />
 `Framework`를 예시로 들면 아래와 같습니다.

![IOS_01](http://docc.upltv.com/uploads/201808/5b88d60fe148e_5b88d60f.png "IOS_01")

### 타사 라이브러리 추가하기
타사와의 연결을 위해 타사 라이브러리를 프로젝트에 추가합니다. <br />
타사 라이브러리는 [이곳](http://doc.upltv.com/en/master/chapters/chapter09.html)에서 다운로드하세요.
현재 UPSDK의 주요 타사는 아래와 같습니다.

![ios_02](http://docc.upltv.com/uploads/201808/5b88d65b062ae_5b88d65b.png "ios_02")
<br>
- **타사 라이브러리는 선택사항이지만 사용하시는 것을 권장합니다. <br />
만약 문제가 발생한다면 담당자에게 문의하여 추가 지원을 받아보세요.**
</br>
### 시스템 참조 라이브러리 추가하기
시스템 참조 라이브러리를 아래 주소에 따라 추가합니다.

TARGETS → General → Linked Frameworks Libraries

- `QuartzCore.framework`
- `MediaPlayer.framework`
- `libsqlite3.tbd`
- `libz.tbd`
- `CoreMedia.framework`
- `CoreGraphics.framework`
- `CFNetwork.framework`
- `WebKit.framework (Optional)`
- `WatchConnectivity.framework (Optional)`
- `SystemConfiguration.framework`
- `StoreKit.framework`
- `Social.framework`
- `MessageUI.framework`
- `JavaScriptCore.framework (Optional)`
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

> **주의: Cocos를 이용하신다면 아래의 라이브러리를 더 추가합니다.**

- `GameController.framework`

<br>

### 프로젝트 구조
#### 1 linker flags를 추가하기

- `TARGETS` → `Build Setting` → `Linking` → `Other Linker Flags` 안에   <br />
`-ObjC`와 `-fobjc-arc`를 추가하시기 바랍니다. (아래 그림을 참조하세요)

![ios_03](http://docc.upltv.com/uploads/201808/5b88d8fb6b6a9_5b88d8fb.png "ios_03")


#### 2 info.plist를 추가하여 http 프로토콜 승인하기

```objective-c
 <key>NSAppTransportSecurity </key>
 <dict>
	 <key>NSAllowsArbitraryLoads </key>
	 <true/>
 </dict>
```

#### 3 info.plist를 추가하여 권한 승인받기
```objective-c
 <key>NSCalendarsUsageDescription </key>
 <string>Some ad content may create a calendar event. </string>
 <key>NSCameraUsageDescription </key>
 <string>Some ad content may access camera to take picture. </string>
 <key>NSPhotoLibraryUsageDescription </key>
 <string>Some ad content may require access to the photo library. </string>
```

> 참고: 디스크립션 설정 언어를 변경하실수 있습니다.
<br>

#### 4 Bitcode
Youlan 과 Domob SDK는 Bitcode를 지원하지 않습니다. <br />
하지만 UPLTV에서는 Bitcode도 지원해드리고 있으니 필요하시면 담당자에게 문의하여 추가 지원을 받아보세요.

<br>

#### 5 Cocos로 빌드 하셨다면, 아래와 같은 `Version` 부분에 Version 값을 입력하시기 바랍니다.

![cocos项目Version字段配置](http://docc.upltv.com/uploads/201709/59afb01ec7612_59afb01e.png "cocos项目Version字段配置")
<br>

### 버전
SDK 버전을 `UPSDKVersion.h`  파일에서 찾으실 수 있습니다.

```objective-c
//sdk version
#define UPSDKVERSION  @"3003"
```
> UPSDK의 현재 버전은 3003 입니다.

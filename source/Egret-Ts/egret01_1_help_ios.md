## iOS

본 액세스 문서는 Egret 5.2.8 버전을 기반으로 두고 있습니다.

### I. SDK 패키지 다운로드하기

먼저 [UPSDK Download](../chapters/chapter09.html "SDKDownLoad")에서 UPSDK Egret TypeScript Plugin 패키지를 다운로드하고 압축해제 하시면, <br />
아래와 같은 세 개의 디렉토리들이 보입니다.

- `UPSDK.framework`: UPSDK 메인 패키지이므로 현재 프로젝트에 추가합니다.
- `UPSDK.bundle`: UPSDK 메인 패키지가 현재 프로젝트에 액세스해야 하므로 외부 파일 리소스를 추가합니다.
- `UpltvEgretTsBridge`: 이 디렉토리에는 현재 Egret TypeScript 프로젝트와 UPSDK AD 인터페이스 호출을 <br />
연동하기 위한 `*.ts` 원본 파일들이 들어 있습니다.

</br>

### II. EgretIDE 프로젝트 설정


1) Egret TypeScript 프로젝트의 `src` 폴더에 `UpltvEgretTsBridge`를 복사하고, `*.ts` 파일만 남겨둡니다.

![](http://docc.upltv.com/uploads/201809/5ba096f858faf_5ba096f8.png)

</br>

### III. Xcode의 프로젝트 설정

#### 1. UPSDK 메인 패키지 및 소스 추가하기

Xcode 프로젝트 디렉토리에 `UPSDK.Framework`, `UPSDK.Bundle` 및 `UpltvEgretTsBridge` 폴더를 동시에 추가합니다.

UpltvEgretTsBridge 폴더안의 공지를 등록합니다.`*.ts` 파일을 제거하고 아래와 같이 Object-C++ 소스는 남겨둡니다.

![](http://docc.upltv.com/uploads/201809/5ba095754ee34_5ba09575.png)


#### 2. 타사 의존성 라이브러리 추가하기

UPSDK는 실행될 때 타사 광고 네트워크사에 의존하므로 수동으로 의존 라이브러리 파일을 프로젝트로 가져와야 합니다.
[여기](http://doc.upltv.com/en/master/chapters/chapter09.html)를 클릭하셔서 타사 의존성 패키지를 추가하시기 바랍니다.

UPSDK는 현재 다음의 타사 광고 네트워크사에 의존합니다.

![Add all third party SDK packages](http://docc.upltv.com/uploads/201709/59afafb9143e9_59afafb9.png)

> 각 광고 네트워크사는 각각의 폴더에 해당하며, 폴더의 이름은 광고 네트워크사명의 접두사와 버전 번호로 <br />
표기하였기에 쉽게 식별할 수 있습니다. 폴더는 다른 의존 라이브러리 또는 원본 파일 외에 지원 리소스 파일을 <br />
포함할 수 있으므로 의존 라이브러리를 해당 지원 리소스와 함께 추가해야 합니다.

현재 프로젝트에 모든 타사 광고 네트워크사를 추가할 필요는 없지만 더 많은 수익을 위해 프로젝트에 가능한 한 많은 <br />
광고 네트워크사 AD 의존성 라이브러리 및 그에 수반되는 자원 패키지를 추가하시기를 권장합니다. <br />
담당자에게 문의하여 추가 지원을 받아보세요.

타사 광고 네트워크사를 추가한 후, XCode 프로젝트 ` Linked Frameworks Libraries `에서 해당 라이브러리 파일을 <br />
확인하시기 바랍니다.

예를 들어, 현재 프로젝트 액세스에서 Applovin, Unity, Vungle, tapjoy 4개의 광고 라이브러리<br />
(`AppLovinSDK.framework`, `UnityAds.framework`, `VungleSDK.framework`, `Tapjoy.framework`)를 추가합니다. <br />
성공적으로 추가한 후의 화면은 아래와 같습니다.

![](http://docc.upltv.com/uploads/201804/5acc6644c33a5_5acc6644.png)

위의 예시에서 Tapjoy AD는 일부 외부 리소스 파일에 액세스해야 하므로 프로젝트에 지원 리소스 <br />
파일을 추가해야 합니다. `TARGETS` → `Build Phases` → `Copy Bundle Resources`

<br>
![](http://docc.upltv.com/uploads/201804/5acc70803fec8_5acc7080.png)


#### 3. 시스템 의존성 추가하기

의존성 라이브러리를 `TARGETS` → `General` → `Linked Frameworks Libraries`에 추가합니다.

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

### IV. 프로젝트 구성
#### 1. 분류 컴파일러 추가하기

- 아래와 같이 모든 타사 의존성 라이브러리 UPSDK.framework를 `-force_load（space）library path`에 추가합니다. <br />
`TARGETS` → `Build Setting` → `Linking` → `Other Linker Flags`

![](http://docc.upltv.com/uploads/201809/5ba21766e18e1_5ba21766.png)

#### 2. HTTP 모드와 호환을 위해 다음 노드를 info.plist에 추가하기

```objective-c
 <key>NSAppTransportSecurity </key>
 <dict>
	 <key>NSAllowsArbitraryLoads </key>
	 <true/>
 </dict>
```
#### 3. 권한을 얻기위해 다음 노드를 info.plist에 추가하기
(AdColony를 사용하는 경우 추가해야 합니다, AdColony를 사용하지 않는 경우에는 추가하지 않아도 됩니다.)

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

### V. Bitcode

Domob은 Bitcode를 지원하지 않습니다. 하지만 UPLTV에서는 Bitcode도 지원해드리고 있으니 필요하시면 <br />
담당자에게 문의하여 추가 지원을 받아보세요.

<br>

### VI. 버전
`UPSDKVersion.h`에서 UPSDK의 버전을 확인하실 수 있습니다.

```objective-c
##define AvidlyAdsSDKVERSION  @"3005"
```

<br>

### VII. 코드 수정하기

#### 1. AppDelegate에서 window 속성 추가하기
```objective-c
@interface AppDelegate : UIResponder <UIApplicationDelegate>
@property (strong, nonatomic) UIWindow *window;
@end

@implementation AppDelegate {
    EgretNativeIOS* _native;
}
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions{
_native = [[EgretNativeIOS alloc] init];
self.window = _native.window;
... ...
}
```
#### 2. 공지 등록
```objective-c
[UpAdsBridgeEgretTs registWithEgretNative:_native];
```

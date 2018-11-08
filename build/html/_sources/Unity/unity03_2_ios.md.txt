## iOS 액세스

### I. 선언

Unity 5.x의 여러 버전 테스트를 통해, Mac에서는 5.1.0 및 상위버전에서만 안정적으로 작동할 수 있다는 것을 <br />
확인할 수 있었습니다. 따라서 5.1.0 및 하위 버전은 적응 테스트를 완료할 수 없습니다. 프로젝트 버전이 5.1.0보다 <br />
낮은 경우 최신 버전으로 업데이트 하시기 바랍니다. 업데이트에 문제가 있으면 불러오기 하시고, <br />
UPLTV의 기술팀 도움이 필요한 경우 UPLTV에 문의하시기 바랍니다.

### II. Unity 프로젝트 Assets 카탈로그 내의 iOS 프레임워크 점검

iOS용 Unity Plugin을 성공적으로 불러온 후 Assets 카탈로그에서 `PolyADSDK/Plugins/IOS/frameworks` 와 `PolyADSDK/Plugins/IOS/resources`의 두 카탈로그가 존재하는지 확인합니다. 카탈로그가 존재하지 않는다면 <br />
Plugin이 성공적으로 불러오기 되지 않았음을 의미합니다.

iOS용 Plugin을 성공적으로 불러오면 아래의 사진과 같습니다.

![aaa](http://docc.upltv.com/uploads/201805/5af573a34f61b_5af573a3.png "aaa")

위 모습에서 보이는 구조에 아래의 파일들이 있는지 확인하시기 바랍니다.

#### 1. UPSDK ADSDK 메인 패키지

`PolyADSDK/Plugins/IOS/frameworks/UPSDKAdsSDK.framework` 는 UPSDK ADSDK의 메인 패키지입니다. <br />
이 파일이 없는 경우 담당자에게 문의하여 추가 지원을 받으시기 바랍니다.

#### 2. 광고 네트워크에 대한 UPSDK ADSDK 의존성

Under `PolyADSDK/Plugins/IOS/frameworks` 카탈로그에 UPSDK ADSDK 메인 패키지를 제외하고 일련의 광고 네트워크 <br />
의존성이 있어야 합니다. 이 프레임워크는 AdCoony.framework 및 UnityAds.framework과 같이 *.framework 형식으로 <br />
존재하고, `*.a` 또는 `*.h` 형식의 OneWaySDK 및 Domob-SDK로 존재하기도 합니다.

#### 3. UPSDK ADSDK 리소스 패키지

`PolyADSDK/Plugins/IOS/resources` 는 리워드 동영상 광고의 UI와 인터렉티브하는 아이콘 리소스와 같이 <br />
UPSDK ADSDK 광고가 사용하는 리소스 파일입니다. 리소스가 부족하면 비정상적 또는 실패로 이어지며, <br />
**수익화에 영향을 미칠 수 있습니다.**

#### 4. XUPorter

XUPorter는 오픈 소스입니다. Xcode에서 내보내는 구성 매개 변수를 동적 편집하는 데 사용되며 의존성 패키지 및 <br />
리소스 패키지를 추가를 포함하고 있습니다. 이 카탈로그의 문서가 부족할 경우 자동으로 조정되지 않습니다. <br />
또한, 리소스를 수동으로 내보낼 수 있습니다. 자세한 내용은 다음 섹션에서 설명합니다.

### Xcode 구성 점검

프로젝트에서 Xcode를 내보내지 못한 경우 아래의 순서에 따라 모든 어댑션 구성을 점검하시기 바랍니다.

#### 1. Deployent Target 설정

`Target` -> `General` 패널에서 `Deployment Target` 값을 7.0 및 그 이상으로 설정합니다. 타사에서 사용하는 <br />
일부 프레임워크에는 7.0 이상이 필요합니다.

예시：
![ios-target](http://docc.upltv.com/uploads/201706/59535d81cf339_59535d81.png "ios-target")

#### 2. 바이너리와 라이브러리 연동 점검

In Build Phases -> Link Binary with Libraries에서 Plugin에 포함된 `*.a` 파일 및 프레임워크가 추가되었는지 점검합니다.

성공적으로 추가한 후에는 다음 스크린샷의 빨간색 상자에 표시된 라이브러리 파일이 추가됩니다.

![ios-lib](http://docc.upltv.com/uploads/201706/595361be1b87e_595361be.png "ios-lib")

`PolyADSDK/Plugins/IOS/frameworks`에 라이브러리 파일이 없는 경우 Xcode 프로젝트를 사용하여 프로젝트에 포함된 `PolyADSDK/Plugins/IOS/frameworks` 파일을 수동으로 추가합니다.

**이 중
`libsqlite3.tbd`, `libxml2.tbd`, `libz.tbd`, `libstdc++.tbd`, `WebKit.framework` 그리고 `JavaScriptCore.framework` 는 <br />
시스템 라이브러리이기에 반드시 추가되어야 합니다. 일부 타사 광고 네트워크는 이러한 네트워크에 의존하기 때문에 <br />
누락된 항목이 있으면 수동으로 추가합니다.**

#### 3. 번들 리소스 점검

`Build Phases` -> `Copy Bundle Resources`에서 Plugin에 포함된 리소스를 추가했는지 여부를 점검합니다.

성공적으로 추가한 후에는 리소스 파일이 추가됩니다. 스크린 샷의 빨간색 상자로 표시된 부분을 참조하시기 바랍니다.

![ios-resources](http://docc.upltv.com/uploads/201706/59536339b4e91_59536339.png "ios-resources")

포함되어야 하는 `PolyADSDK/Plugins/IOS/resources` 가 없는 경우 Xcode 프로젝트에 `PolyADSDK/Plugins/IOS/resources`의 <br />
리소스를 수동으로 추가합니다.

#### 4. 기타 링커 플래그 점검
`Build Settings` -> `Linking -> Other Linker Flags` 순서에 따라 점검합니다. <br />
`-ObjC` 및 `-fobjc-arc` 구성이 추가되었는지 점검합니다.

성공적으로 추가 되었다면 아래의 스크린 샷의 빨간색 상자로 표시된 부분에 구성 매개 변수가 추가됩니다. <br />
누락된 경우 수동으로 추가하시기 바랍니다.

![ios-link-set](http://docc.upltv.com/uploads/201706/59536443253fc_59536443.png "ios-link-set")

#### 5. 일부 Unity 버전의 호환성 문제 해결

- 5.4.2 ~ 5.4.0 버전에서, 프레임워크(스크린 샷에서 빨간색으로 표시되어 있는)를 불러오지 못한 경우 <br />
다음 카탈로그에 있는 코드 라이브러리 및 리소스 라이브러리를 수동으로 프로젝트로 불러오기 합니다.

> `PolyADSDK/Plugins/IOS/frameworks` <br />
`PolyADSDK/Plugins/IOS/resources`

- 5.3.1 및 기타 버전에의 il2cpp-config.h 에서 noreturn 정의 오류가 발생하면, 아래의 방법을 참조하시기 바랍니다.

```cpp
 #define NORETURN __declspec(noreturn)
Replace
#define NORETURN __attribute__((noreturn))
```
- 5.1.4 및 기타 버전에서 **Bitcode를 포함하지 않는**  에러가 발생한다면, Bitcode 사용에 ‘NO’를 설정합니다.

#### 6. 프로젝트 정리, 리컴파일

점검을 마치면 Product -> Clean 후에 컴파일 합니다.

에러가 여전히 존재하는 경우, 스크린샷 및 로그와 같은 에러 내용을 UPLTV 기술 지원팀 담당자와 상의하세요.

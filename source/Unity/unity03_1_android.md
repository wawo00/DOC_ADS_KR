## Android 액세스

### I. 선언

Android 플랫폼, Unity 5.0.0 및 상위 버전에 지원합니다.

### II. Unity 프로젝트에서 Asset 카탈로그 확인

Android용 Unity Plugin 불러오기가 완료되면, Asset 카탈로그 아래에 `PolyADSDK/Plugins/Android`가 있는지 <br />
확인하시기 바랍니다.

> 3.0.02 및 하위 Plugin 버전에서 불러오기를 하면 아래의 화면과 같습니다.

![import](http://docc.upltv.com/uploads/201804/5acc569aeb642_5acc569a.png "工程结构")

- `libs_res`: 이 디렉토리에는 `AvidlyAdsSdk_2.0.xx_dex.aar` 및 `support-v4-24.1.1.aar`, <br />
그리고 `proguard-project.txt` 파일이 포함되어 있습니다. support-v4 라이브러리를 추가해야 합니다. <br />
V24.1.1 버전은 Plugin이 권장하는 버전입니다. 프로젝트에 다른 상위 수준의 v4 라이브러리가 있는 경우 권장 버전으로 <br />
직접 교체할 수 있습니다.
- `thirdlibs`: `thirdlibs/aar`는 appcompat-v7-24.1.1.jar 과 같이 동적 로드를 지원하지 않는 <br />
일부 네트워크 라이브러리를 참조하는 데 사용되는 디렉토리입니다.
- `avidly_android`: 이 디렉토리에는 런타임에 동적 로드를 위해 `dex_xxx.jar`로 명명된 <br />
많은 광고 네트워크가 포함되어 있습니다.


> 3.0.03 및 상위 Plugin 버전을 성공적으로 가져오면 `Assets` 카탈로그에 다음과 같이 나타납니다.

![工程结构](http://docc.upltv.com/uploads/201805/5b026a7687a70_5b026a76.jpeg "55333")

3.0.03 및 상위 Plugin 버전에서는 액세스 단계를 간소화하기 위해 `avidly_android`를 제거하고 <br />
 `AvidlyAdsSdk_x.x.xx_dex.aar`를 `UPAdsSdk_3.0.xx_dex.aar`로 변경합니다.

### III. 유니티 전용 버전 적용

1. 5.1.1 버전에서는 컴파일-빌드-패키지하는 동안 Unity는 .arr 파일에서 Armeabi structure의 에러를 <br />
발견할 수 없습니다. <br />
(디 언아카이버(The Unarchiver) 프로그램을 사용하여 위 파일을 열어서 armeabi 폴더를 직접 삭제하셔야 합니다.) <br />

2. 5.0.0 버전에서는 apk 패키징 프로세스 동안, AndroidManifest.xml합병 후 권한을 잃어버렸다면 <br />
\Assets\Plugins\Android 페이지에 AndroidManifest.xml을 추가해야 합니다. <br />
[5.0.0 AndroidManifest 다운로드 페이지](../chapters/chapter09.html "SDKDownLoad")

> 필요한 파일을 찾지 못한 경우 UPLTV의 기술 지원팀 담당자와 상의하세요.

### IV. Unity 프로젝트의 컴파일 에러

Unity 컴파일 에러 해결 방법은 [이곳](../Android/android01_3_help_faq.html "常见问题")에서 Android 결합 문제 솔루션을 참조하시기 바랍니다.

### V. Unity Android proguard 구성

proguard 구성이 있는 프로젝트에, `proguard-project.txt`를 proguard 파일에 복사하여 proguard가 <br />
프로그램 충돌의 원인이 될 수 있는 잘못된 패키지 이름을 가져오지 않도록 합니다.

### VI. Unity 프로젝트의 SDK과도한 적용

Android에는 65535 제한 문제가 있어서 프로젝트가 타사의 *.jar 또는 *.aar의 과도한 파일들을 불러오기 할 때 <br />
하위 패키지가 작동하지 않을 수 있습니다. 이러한 문제를 해결하도록 불필요한 광고 타사 라이브러리를 삭제하고 <br />
아래의 제안 사항들을 이행하시기 바랍니다.

- Plugin 버전이 2.0.24보다 낮은 경우 최신 버전으로 업데이트합니다. 2.0.24 및 상위 버전은 <br />
Android용 dex dimatc-loading 모델을 가져와 타사 라이브러리에서 불러온 aar 파일과의 충돌을 방지합니다.
- 문제가 해결되지 않으면 `thirdlibs/aar/fb/` 파일을 삭제해 보시기 바랍니다.
- 여전히 문제가 해결되지 않았다면 UPLTV의 기술 지원팀 담당자와 상의하세요.메소드를 줄이기 위해 <br />
`thirdlibs/aar/china/`에서 arr 파일을 삭제할지에 대한 여부를 결정하는데 도움을 드리겠습니다.

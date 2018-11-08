## Eclipse

### I. UPSDK 디렉토리 구조
Eclipse 또는 앤트(Ant)를 사용하여 Android 프로젝트를 빌드하면, Android Studio를 사용할 때보다 <br />
훨씬 복잡해질 수 있습니다. UPSDK Eclipse 버전을 다운로드 하고 압축 해제 한 후의 폴더들은 아래의 그림과 같습니다.

![](http://docc.upltv.com/uploads/201808/5b8756d508084_5b8756d5.png)


UPSDK Eclipse의 메인 패키지는 `upsdk_ads` 폴더에 있습니다. 위의 스크린샷에서 볼 수 있는 `upsdk_ads` 가 <br />
Eclipse 프로젝트의 메인 패키지이며, 이를 프로젝트에 반드시 추가해야 합니다.

### UPSDK Eclipse의 메인 패키지를 프로젝트로 복사하기
`upsdk_ads`디렉토리에는 `libs`,`assets`그리고`res` 와 다른 몇 개의 파일들로 구성되어 있습니다. <br />
Eclipse 메인 프로젝트로 디렉토리 내의 모든 파일들을 복사하세요.

>*.so로 된 파일 이외의 파일중에서, 복사 후에 중복된 파일명을 발견했다면 신중히 확인하고 실행하시기 바랍니다. <br />
**이름이 중복된 파일이 UpAdsSdk 원본 파일인 경우에는 덮어쓰기를 합니다.** 상기된 파일명 중복이 아닌 경우, <br />
개발자 본인의 경험에 따라(예를 들어, res/values/ 디렉토리에 있는 파일명 변경) 조치를 취합니다. <br />
UPLTV의 기술 팀에 문의하여 문제 해결 및 지원 서비스를 받을 수도 있습니다.

### III. 다른 의존성 추가하기
일부 타사의 SDK는 타사의 오픈소스 라이브러리에 대한 의존을 필요로 하기에 직접 프로젝트에 포함시켜야 합니다. <br />
이러한 라이브러리 파일들은 다운로드한 폴더 중 `xxx_ads`폴더 형태로 존재하게 됩니다. <br />
UPSDK는 다른 네트워크와 독립적이면서도 긴밀하게 결합되어 있으므로, 일부 타사를 가져오고 싶지 않은 경우에는, <br />
희망 타사 파일만 별도로 선택할 수도 있습니다.

`batmobi_ads` 폴더의 경우, Mobvista 광고로 프로젝트를 채우고 싶다면 `batmobi_ads/libs/`와 <br />
`batmobi_ads/res/` 디렉토리에 있는 관련 파일들을 프로젝트 내 `libs`와 `res` 디렉토리로 복사해야 합니다. <br />
그리고 `AndroidManifest_Inmobi`의 내용을 프로젝트에 있는`manifest.xml`파일로 추가합니다.

**프로젝트에서 UPSDK가 의존을 필요로 하는 타사 라이브러리를 가져올 경우, 아래의 권장 사항을 따라주시기 바랍니다. <br />
그렇지 않으면, 필요한 지원이 부족하다는 사유로 시스템에서 충돌이 발생할 수 있습니다.**

#### IV. Android 지원 라이브러리 추가하기
일부 타사에서는 `support`를 필요로 하므로, 이를 프로젝트에 포함시켜야 합니다. <br />
UPLTV는 `android_support_library/libs` 폴더에 있는 `xxx.jar`를 제공합니다. <br />
파일들을 `libs` 폴더로 추가하시기 바랍니다.

**Android 지원 라이브러리는 강제로 지정된 의존성이므로, 이를 프로젝트에 포함시키지 않으면, <br />
게임에서 충돌이 발생할 수 있습니다**

#### V. Google 광고 SDK 추가하기
전 세계에 걸쳐 프로젝트를 게시하는 경우, Google 광고 지원을 포함할 것을 권장합니다. <br />
UPLTV는 `admob_xxx/libs` 디렉토리에 있는 `play-services-xxx.jar` 관련 파일을 제공합니다.

그리고 AndroidManifest.xml에 아래와 같이 정의가 포함되어 있는지 확인하시기 바랍니다.

	<!-- admob -->
	<meta-data
		android:name="com.google.android.gms.version"
		android:value="@integer/google_play_services_version"/>

	<activity
		android:name="com.google.android.gms.ads.AdActivity"
		android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"
		android:multiprocess="true"
		android:theme="@android:style/Theme.Translucent"/>
	<!-- admob -->


### VI. AndroidManifest.xml 파일 수정하기
`AndroidManifest.xml` 파일의 내용을 프로젝트의 관련 위치에 복사합니다.

### VII. Proguard 설정 수정하기
프로젝트에서 `proguard`를 사용하는 경우, `proguard-project.txt`의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

### VIII.  Demo 프로젝트
광고 SDK를 더 빠르고 쉽게 결합할 수 있도록 간편한 [Demo 프로젝트](https://upsdk-korean.readthedocs.io/ko/master/Android/android08_demo.html)를 제공하고 있습니다.

## FAQ

### I. Android Studio AndroidManfiest.xml 충돌 선언

#### 1. com.google.android.gms.version 충돌 또는 삭제 오류

**Android Studio** 버전에서 이와 같은 충돌을 방지하기 위해, <br />
2.0.21 Studio 버전부터 UPSDK에서는 AndroidManfiest.xml에서 아래의 선언을 삭제하였습니다.

```asp
<!-- admob -->
<meta-data

            android:name="com.google.android.gms.version"

            android:value="@integer/google_play_services_version"/>
```

따라서, 2.0.21 이전 버전에서 gms.version 충돌이 발생하는 경우, 당사의 기술 지원팀으로 문의하시기 바랍니다.

하지만, **Android Eclipse** 버전에서는 AndroidManfiest.xml에 아래의 선언을 추가해야 합니다.

```asp
<!-- admob -->
<meta-data
            android:name="com.google.android.gms.version"
            android:value="@integer/google_play_services_version"/>
```

특히, `res/values` 콘텐츠에 있는 `xml` 형식의 파일에 `Google_play_services_version`이 정의되어 있는지 <br />
확인하시기 바랍니다. 일반적으로 UPSDK에서는 `version_ad.xml` 파일에서 이와 같은 정의를 사용하고 있으니, <br />
이에 대한 정의가 없는 경우, `version_ad.xml`를 `res/values` 디렉토리에 복사하시기 바랍니다.

`version_ad.xml` 내용：
```asp
<?xml version="1.0" encoding="utf-8"?>
<resources>
	// defined the value of gms version, the kind of integer
	// if the current version is not 9080000, please change to the correct version.
    <integer name="google_play_services_version">11020000</integer>
</resources>
```

#### 2. 액티비티 충돌 또는 삭제 오류
Android Studio 버전, 특히 Studio 2.0.21 상위 버전의 UPSDK에서는 AndroidManfiest.xml에서 <br />
Admob 및 Facebook 광고에 의존하는 액티비티 선언을 삭제하였습니다.

```asp
<!-- admob -->

<activity
android:name="com.google.android.gms.ads.AdActivity"
            android:configChanges="keyboard|keyboardHidden|orientation|screenLayout|uiMode|screenSize|smallestScreenSize"
android:multiprocess="true"
android:theme="@android:style/Theme.Translucent"/>
<!-- admob -->


<!-- facebook -->
<activity
android:name="com.facebook.ads.InterstitialAdActivity"
android:configChanges="keyboardHidden|orientation|screenSize"
android:multiprocess="true"/>
```


해당 선언을 삭제한 이유는 Admob과 Facebook이 Studio 프로젝트에서 정상적으로 작동할 때 aar 형식으로 가져온 후, <br />
관련 구성 요소를 `AndroidManfiest.xml`에서 선언하기 때문입니다.

반면, jar 패키지로 결합되는 Eclipse 프로젝트와 Studio 프로젝트는 메인 프로젝트의 AndroidManfiest.xml에 <br />
액티비티 선언을 직접 추가해야 합니다.

### II. Admob gms 충돌
#### 1. Facebook 광고가 Admob 광고에 의존하여 발생하는 충돌
Facebook 광고를 컴파일하여 온라인에서 audience-network-sdk를 업데이트할 때, <br />
audience-network-sdk와 관련된 aar 파일 뿐만 아니라 Google gms 저장소에 있는 아래의 파일도 함께 다운로드합니다.

- play-services-ads-8.4.0
- play-services-basement-8.4.0

Google gms 서비스가 다른 버전(예: 10.0.1 버전)에서 의존성을 업데이트되는 경우, Studio Gradle은 자동으로 <br />
위 8.4.0 버전을 모두 삭제합니다. 하지만 8.4.0이 아닌 Google gms 서비스를 **Local**에서 불러오기를 하면 프로젝트에 <br />
play-services-ads 및 play-services-basement가 생성되어 **gms에 대해 클래스 반복 인용 충돌이 발생합니다.**

따라서, 아래의 상황들이 발생되지 않도록 주의하시기 바랍니다.
- Google gms 서비스와 Facebook Audience Network SDK를 함께 업데이트하기
- Google gms 서비스와 Facebook Audience Network SDK를 함께 aar로 가져오기
- Google gms 서비스를 온라인에서 업데이트할 때, Facebook Audience Network SDK를 aar이나 jar 패키지로 가져오기

#### 2. 기타 충돌

Admob 광고에서는 gms 플레이 서비스 지원이 필요합니다. 따라서 Admob 광고 지원을 필요로 하는 프로젝트의 경우, <br />
관련 gms 의존성과 구성을 추가해야 합니다.

Admob을 지원하는 UPSDK는 아래의 gms 플레이어 서비스 라이브러리에 의존해야 합니다.
- play-services-ads-x.x.x
- play-services-ads-lite-x.x.x
- play-services-base-x.x.x
- play-services-basement-x.x.x
- play-services-tasks-x.x.x

> 현재 UPSDK에 장착된 gms 버전은 `play-services-ads:11.0.4` 입니다. 메인 프로젝트의 gms 버전과 다른 경우, <br />
상위 gms 버전을 유지하세요. 불필요한 오류를 미연에 방지하려면, 버전에 관계없이 앞서 언급한 5개의 의존성을 항상 같은 버전으로 <br />
유지해야 합니다. 그렇지 않은 경우, 클래스 반복 인용 충돌이 발생할 수 있습니다.


### III. xxx.so 누락 문제
`AvidlyAdsSDK_x.x.x.aar` 및 `adcolony_ads.aar`에서는 아래와 같이 3가지 종류의 CPU 타입 `*.so` 파일을 제공합니다.

- armeabi
- armeabi-v7a
- x86

프로젝트에서 상기 3가지 파일보다 적거나, 많거나, 또는 이와 다른 사항을 지원하는 경우<br />
(예: Cococs 엔진에서 개발한 게임은 일반적으로 armeabi만 지원함), 프로그램 실행 시 <br />
아래와 같은 오류가 발생할 수 있습니다.


```
java.lang.UnsatisfiedLinkError:
Couldn't load cocos2dlua from loader dalvik.system.PathClassLoader
    [DexPathList[[zip file "/data/app/org.cocos2dx.xxxxx-1.apk"],
    nativeLibraryDirectories=[/data/app-lib/org.cocos2dx.xxxxx-1,
    /vendor/lib, /system/lib, /data/datalib]]]: findLibrary returned null
              at java.lang.Runtime.loadLibrary(Runtime.java:358)
              at java.lang.System.loadLibrary(System.java:555)
              at org.cocos2dx.lib.Cocos2dxActivity.onLoadNativeLibraries(Cocos2dxActivity.java:248)
              at org.cocos2dx.lib.Cocos2dxActivity.onCreate(Cocos2dxActivity.java:264)
              at org.cocos2dx.lua.AppActivity.onCreate(AppActivity.java:57)

```
이유는 *.so 파일이 CPU 프레임워크와 정렬되지 않았기 때문입니다.

문제를 해결하는 두가지 방법:
1. Gradle 프로젝트의 경우, 앱 카탈로그의 gradle.build 파일에 지원 프레임워크를 직접 추가합니다.

    ```asp
    defaultConfig {

            ndk {
                // The assumption is if the app support four kinds of CPU as followed
                abiFilters 'armeabi' ,'armeabi-v7a', 'x86', 'arm64-v8a'
            }

        }
    ```
    > abiFilters에 추가된 CPU 프레임워크는 현재 프로젝트를 지원하는 프레임워크입니다.

2. 수정할 수 없는 Eclipse 및 `grandle.build` 프로젝트의 경우, 라이브러리 카탈로그에 있는 유효하지 않은 <br />
CPU 프레임워크를 삭제합니다. aar 파일의 경우, 압축 해제하여 불필요한 CPU 프레임워크를 삭제합니다.

## Android Studio

### I. UPSDK CppPlugin의 구조

Android Studio나 Gradle 빌드 프로젝트에 따라 UPSDK는 다른 메인 프로젝트에서 `*.aar`형식으로 가져와야 합니다. <br />
([Android-CPPSDK](../chapters/chapter09.html) UPSDK CppPlugin)를 다운받아 압축해제 하시면 아래의 그림과 같은 <br />
디렉토리 구조를 보실 수 있습니다.

![as-1-1](http://docc.upltv.com/uploads/201805/5afd2c552eab2_5afd2c55.png "as-1-1")

>UPSDK CppPlugin 메인 패키지명은 `UPAdsSdk_Cpp_x.x.xx_dex.aar`입니다.

- `Android Studio`: 이 디렉토리에는 Android Studio에 필요한 광고 의존성 라이브러리 파일들이 포함되어 있습니다.
- `cpp`: 이 디렉토리에는 현재 Cocos2d-X cpp 프로젝트를 UPSDK 인터페이스로 연동하기 위한 일부 <br />
*.cpp 소스 파일들이 포함되어 있습니다.
- `Eclipse`: 이 디렉토리에는 일부 jar 및 리소스 파일이 포함되어 있습니다. Android Studio를 사용하여 <br />
apk를 빌드하는 경우 이 디렉토리를 무시하시기 바랍니다.
- `proguard-project.txt`: 프로젝트에서 `proguard`를 사용하는 경우, 파일의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

### II. UPSDK 메인 패키지 가져오기

#### 1. UPSDK 파일 추가하기

상기 설명에 따라 `UPAdsSdk_Cpp_x.x.xx_dex.aar`라는 파일을 다운로드 받은 파일 디렉토리에서 찾아서 프로젝트의 <br />
`libs` 디렉토리에 추가합니다. (참고: 이 디렉토리가 없는 경우 src와 동일한 디렉토리를 생성합니다.) <br />
추가한 후의 구성은 아래와 같습니다.

![as-2-1](http://docc.upltv.com/uploads/201805/5afd2d4e483b6_5afd2d4e.png "as-2-1")
> `UPAdsSdk_Cpp_3.0.03_dex`는 참조용입니다.

libs 디렉토리의 aar 패키지가 프로젝트에 의해 올바르게 참조되었는지 확인하기 위해 `app` 디렉토리의 <br />
`build.gradle`파일을 확인하여 다음 매개 변수를 추가합니다.

```groovy
repositories {
        flatDir {
             dirs 'libs'
        }
}

```
마지막으로 의존성 범위에 aar을 추가합니다.

```groovy
dependencies {
        // UPAdsSdk_Cpp_3.0.03_dex를 실제 파일 명과 치환합니다.
        compile(name: 'UPAdsSdk_Cpp_3.0.03_dex', ext: 'aar')
}
```

#### 2. `build.gradle`에서 minSdkVersion 및 targetSdkVersion 확인하기

UPSDK를 사용하려면 `minSdkVersion`이 15 보다 크고, `targetSdkVersion`이 26 보다 작아야 합니다. <br />
이는 `build.gradle` 에서 확인하시기 바랍니다.

샘플：

```groovy
 defaultConfig {
        minSdkVersion 16
        targetSdkVersion 26
}
```
cocos2dx-3.X 프로젝트는 `PROP_MIN_SDK_VERSION` 및 `PROP_TARGET_SDK_VERSION`의 디폴트 구성을 <br />
`gradle.properties`에서 실행하므로 이 두 개의 값만 수정하면 됩니다.

권장 값은 다음과 같습니다.
```
 PROP_MIN_SDK_VERSION=16
 PROP_TARGET_SDK_VERSION=26
```

#### 3. *.so 파일이 정렬되었는지 확인하기

`UPAdsSdk_Cpp_x.x.xx_dex.aar`에는 세 가지 유형의 *.so 파일이 있습니다.

- armeabi
- armeabi-v7a
- x86

abiFilters 구성을 `gradle.build` 파일에 프로젝트에서 지원하는 CPU 유형에 따라 추가합니다. <br />
그리고 파일들이 정렬되어 있는지 확인합니다.

현재 프로젝트가  armeabi-v7a만 지원하는 경우 아래의 수정 사항을 참조하시기 바랍니다.

```
defaultConfig {
        ndk {
            abiFilters 'armeabi-v7a'
        }
 }
```

>AbiFilters `armeabi-v7a'`는 이 앱이 armeabi-v7a 아키텍처에 기반을 둔 디바이스에서 지원됨을 의미합니다.

### III. 광고 네트워크 추가하기

#### 1. Google Ads SDK 추가하기

**샘플 코드**
`build.gradle`에서 컴파일 명령을 통해 Google의 원격 저장소에서 gms Play-service15.0.1 패키지를 <br />
다음과 같이 다운로드합니다.

    dependencies {
        compile 'com.google.android.gms:play-services-ads:17.2.0'
    }


> 프로젝트에 이미 다른 버전의 Google Play 서비스가 있는 경우 상위 버전을 사용하시기 바랍니다.

#### 2. 기타 광고 네트워크 추가하기

더 많은 수익을 위해 프로젝트에 가능한 한 많은 광고 라이브러리를 추가합니다. 다음 방법을 참조하여 <br />
`Android Studio/aar`의 `xxx_ads.aar`파일을 프로젝트에 추가합니다.

###### 중국을 제외한 전 세계 광고

build.gradle은 다음과 같습니다.

```groovy
dependencies {
    //다른 광고 네트워크 라이브러리
    //gson-2.7.jar은 android_support_library 명의 폴더 안에 있습니다.
    compile(name: 'gson-2.7', ext: 'jar')
    compile(name: 'facebook_ads', ext: 'aar')
    compile(name: 'facebook_exo_player', ext: 'aar')

    compile(name: 'mobvista_ads', ext: 'aar')
    compile(name: 'unity_ads', ext: 'aar')
    compile(name: 'vungle_ads', ext: 'aar')
    compile(name: 'chartboost_ads', ext: 'aar')
    compile(name: 'ironsource_ads', ext: 'aar')

    compile(name: 'adcolony_ads', ext: 'aar')
    compile(name: 'applovin_ads', ext: 'aar')
    compile(name: 'playable_ads', ext: 'aar')
    compile(name: 'tapjoy_ads', ext: 'aar')
}
```

###### 중국에서만의 광고

build.gradle은 다음과 같습니다.

```groovy
dependencies {
    //다른 광고 네트워크 라이브러리
    //gson-2.7.jar은 android_support_library 명의 폴더 안에 있습니다.
    compile(name: 'gson-2.7', ext: 'jar')
    compile(name: 'centrixlink_ads', ext: 'aar')

    compile(name: 'mobvista_ads', ext: 'aar')
    compile(name: 'vungle_ads', ext: 'aar')
    compile(name: 'chartboost_ads', ext: 'aar')

    compile(name: 'inmobi_ads', ext: 'aar')
    compile(name: 'oneway_ads', ext: 'aar')
    compile(name: 'playable_ads', ext: 'aar')
}
```

###### 전 세계 광고

위의 두 파일을 하나의 파일로 병합하시기 바랍니다.

### IV. Android 지원 라이브러리 추가하기

모든 광고 네트워크사들은 `android.support.xxx`가 필요하기에 프로젝트로 가져옵니다. <br />
프로젝트에서 Android Studio를 사용하는 경우, `build.gradle`파일을 수정하여 추가할 수 있습니다.

UPSDK의 3.0.04 버전부터 Admob 및 기타 광고 네트워크의 업그레이드로 인해, Android 지원 라이브러리의(v4 및 v7를 <br />
포함하여) 26.1.0 버전을 사용하는 것을 권장합니다. 특별한 경우, Android 지원 라이브러리의 v25.3.1과 같은 <br />
하위 버전을 사용하실 수도 있습니다.

#### 1. Android 지원 라이브러리 가져오기

`build.gradle`에서 컴파일 명령을 통해 Google의 원격 저장소에서 지원 라이브러리를 다음과 같이 다운로드합니다.

```groovy
dependencies {
    //핵심 라이브러리
    compile(name: 'UPAdsSdk_Cpp_3.0.04_dex', ext: 'aar')
    //지원 라이브러리
    compile 'com.android.support:recyclerview-v7:26.1.0'
    compile 'com.android.support:support-v4:26.1.0'
    compile 'com.android.support:appcompat-v7:26.1.0'
    compile 'com.android.support:support-annotations:26.1.0'
    compile  'com.android.support:customtabs:26.1.0'
    compile 'com.android.support:cardview-v7:26.1.0'
}

```

**【주의】Android Support 지원 라이브러리를 프로젝트에 가져와야 합니다.**

### V. *.cpp  파일 추가하기

#### 1. 프로젝트에 파일 추가하기
UPSDK는 *cpp 소스 파일을 통해 네이티브 인터페이스(Native Interface)에 대한 크로스 플랫폼(Cross-Platform)을 <br />
구현하므로 `cpp/upltv`에 있는 모든 *.cpp 및 *.h 파일을 현재 프로젝트에 복사하시기 바랍니다.

Cocos2d-x 3.16 버전을 Classes 폴더에 복사할 수 있습니다. 버전 차이가 있는 경우 수정을 참조하시기 바랍니다. <br />
복사 후의 디렉토리는 아래와 같습니다.

![classes](http://docc.upltv.com/uploads/201804/5acacf5c67cbc_5acacf5c.png "classes")
>본문은 cocos2dx-3.16을 바탕으로 서술하고 있습니다. 디렉토리가 일치하지 않으면 <br />
담당자에게 문의하여 추가 지원을 받아보세요.

### 2. jni 디렉토리에서 android.mk 수정하기

프로젝트의 jni 디렉토리에 있는 android.mk 파일을 엽니다.

**UpltvAndroid.cpp,CocosUpLtv.cpp,UpltvBridge.cpp** 파일을 **LOCAL_SRC_FILES**로 복사합니다. <br />
Cocos2d-X 3.16을 예시로, 추가 방법은 다음과 같습니다. 기타 버전의 경우 해당되는 경로를 수정합니다.

```groovy
LOCAL_SRC_FILES := $(LOCAL_PATH)/hellocpp/main.cpp \
                   $(LOCAL_PATH)/../../../Classes/AppDelegate.cpp \
                   $(LOCAL_PATH)/../../../Classes/HelloWorldScene.cpp \
                   $(LOCAL_PATH)/../../../Classes/upltv/UpltvAndroid.cpp \
                   $(LOCAL_PATH)/../../../Classes/upltv/CocosUpLtv.cpp \
                   $(LOCAL_PATH)/../../../Classes/upltv/UpltvBridge.cpp
```
### Ⅵ. Proguard 설정 수정하기

프로젝트에서 `proguard`를 사용하는 경우, `proguard-project.txt`에서 파일의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

### Ⅶ. 65535 제한 문제 해결

UPSDK 액세스의 메소드 수가 65535를 초과하여 빌드할 수 없는 경우 `MultiDex`를 사용합니다. 자세한 내용은 <br />
[MultiDex Scheme](../Android/android02_65535.html "Fix up 65535")을 참조하시기 바랍니다.


### Ⅷ.Demo 프로젝트

광고 SDK를 쉽고 빠르게 결합할 수 있도록 [Demo 프로젝트 ](../Cocos2d-X_Cpp/cpp03_7_sample_demo.html)를 제공합니다.

## Android Studio

### I. UPSDK 디렉토리 구조
Android Studio나 Gradle 빌드 프로젝트에 따라 UPSDK는 다른 메인 프로젝트에서 `*.aar`형식으로 가져와야 합니다.<br />
UPSDK Studio를 다운받아 압축해제 하시면 아래의 그림과 같은 목록 구조를 보실 수 있습니다.

![as_91](http://docc.upltv.com/uploads/201808/5b7fdca87b9ae_5b7fdca8.png "as_91")

#### 1. UPSDK 메인 패키지
위의 스크린샷에서 볼 수 있는 `UPAdsSdk_x.x.xx.aar` 이 UPSDK의 메인 패키지이며, <br />
프로젝트에 이를 반드시 추가해야 합니다.

#### 2. UPSDK 네트워크 의존성 추가하기
UPSDK는 다른 네트워크와 독립적이면서도 긴밀하게 결합되어 있으므로, <br />
일부 타사 파일을 가져오고 싶지 않은 경우에는 가져올 타사 파일만 별도로 선택하여 앱의 용량을 줄일 수도 있습니다.

Admob및 Facebook을 제외한 네트워크의 의존성은 `xxxx_ads.aar`형식으로 존재합니다.

#### 3. Admob및 Facebook의 광고 의존성
UPSDK 로컬 파일 중, Admob 및 Facebook에 인터넷 연결 상태가 좋지 않거나 <br />
예기치 않은 문제가 발생할 경우를 대비하여 aar 의존성을 제공합니다. <br />
그럼에도 불구하고, UPLTV는 Admob및 Facebook의 long-distance warehouse의 gradle의 의존성을 <br />
온라인으로 업데이트하는 것을 권장합니다.

> Admob및 Facebook의 결합 방법과 관련하여, 본 섹션 후반부에 소개될 예정입니다.

### II. Android Studio의 Gradle을 사용하여 UPSDK의 메인 패키지를 가져오기

위의 설명을 참고하여 다운로드된  `UPAdsSdk_x.x.xx.aar`파일을 프로젝트 `libs`디렉토리에 추가하세요. <br />
Studio 프로젝트의 효과는 아래와 같습니다:

![as_02](http://docc.upltv.com/uploads/201808/5b7fddd4a0a05_5b7fddd4.png "as_02")

> `UPAdsSdk_3.0.03.aar` 를 실제 파일 이름으로 바꿉니다.


프로젝트에 제대로 aar 패키지가 가져오기 되었는지 `app` 디렉토리의 `build.gradle` 파일 <br />
혹은 폴더에서 아래의 구성 매개변수를 확인합니다.

    repositories {
        flatDir {
            dirs 'libs'
        }
    }

컴파일 메소드에 aar을 추가합니다.

    dependencies {
    // your other setting
    // Please replace UPAdsSdk_3.0.03 with the real file name
     compile(name: 'UPAdsSdk_3.0.03', ext: 'aar')
    }

UPSDK 패키지가 성공적으로 구성되었습니다. gradle 컴파일 작업을 기다립니다. <br />
마지막으로 **기타 의존성**을 추가합니다.

> 우수하고 강력한 미디에이션 플랫폼인 UPSDK는 타사의 런타임 라이브러리(Runtime library)와 유연하게 호환하여 <br />
최대한의 수익을 낼 수 있도록 도와 드립니다. 따라서 UPSDK를 통하여 최대한의 수익을 위해 타사 의존성을 <br />
추가하시기 바랍니다.

Android Studio의 Gradle을 사용하여 프로젝트를 빌드하는 경우 먼저 `for_Studio`디렉토리에서 <br />
모든 `UPAdsSdk_x.x.xx.aar`파일을 프로젝트로 가져와야 합니다. 디렉토리에 있는 각 `xxx_ads.aar`파일은 <br />
UPLTV와 제휴되어 있는 광고 네트워크를 의미합니다.

저희는 항상 전세계에서 최고의 광고 네트워크사를 선택하므로, 모든 네트워크를 프로젝트로 가져올 수 있습니다. <br />
또한, 라이브러리가 너무 많아짐에 따라 커지는 게임의 용량 혹은 기타 사유로 몇 개의 네트워크를 제거하실 수 있습니다.  

### III. 광고 네트워크 의존성 추가하기

일부 광고 네트워크의 SDK는 광고 네트워크사의 오픈소스 라이브러리에 의존합니다. <br />
Android Studio가 만든 프로젝트는 아래의 방법을 통해 종속 타사 라이브러리를 가져올 수 있습니다.

**기본적으로 프로젝트의 종속된 타사 라이브러리를 모두 가져오는 것을 권장하지만 <br />
경우에 따라 프로젝트의 크기를 줄이기 위해 추가할 광고 네트워크 라이브러리를 선택할 수 있습니다. <br />
그러나, 추가한 광고 네트워크의 라이브러리가 현재 표시된 광고의 외부 조건을 완전히 충족시켜야 합니다. <br />
그렇지 않을 경우 충돌이 발생할 수 있습니다.**

일부 광고 네트워크에서 지원을 제거하고 싶지만 방법을 모르는 경우 UPLTV의 기술 팀에 문의하여 <br />
문제 해결 및 지원 서비스를 받을 수도 있습니다.

#### 1. 광고 네트워크 *.aar 추가하기
다운로드한 타겟 파일 중  `xxx_ads.aar`라는 파일은 사용 중인 광고 네트워크의 종속 파일입니다. <br />
**UPSDK aar 파일 가져오기**를 참고하여 프로젝트에 추가합니다.

#### 2. 다른 외부 의존성 추가하기
일부 광고 네트워크에서  Android Api 추가 지원을 요구하기 때문에 다음과 같이 외부 의존성을 추가해야 합니다.

#### 1.  Gradle에 jcenter 추가하기
메인 프로젝트의 `build.gradle`파일을 찾아서  `repositories`섹션에 `jcenter` 를 추가합니다.

```groovy
buildscript {
    repositories {
        mavenCentral();
        // ******** jcenter 추가하기********
        jcenter()
        maven {
            url 'https://maven.google.com/'
            name 'Google'
        }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.1.3'
    }
}

allprojects {
    repositories {
        jcenter()
        maven {
            url 'https://maven.google.com/'
            name 'Google'
        }
    }
}
```

#### 2.  Android 지원 라이브러리 추가하기
Admob 등의 많은 광고사에서 요구하기 때문에 `android.support.xxx`를 프로젝트로 불러오기 해야합니다. <br />
프로젝트에서 Android Studio를 사용하는 경우, `build.gradle`파일을 수정하여 추가할 수 있습니다.
```groovy
dependencies {
    compile 'com.android.support:recyclerview-v7:26.1.0'
    compile 'com.android.support:support-v4:26.1.0'
    compile 'com.android.support:appcompat-v7:26.1.0'
    compile 'com.android.support:support-annotations:26.1.0'
    compile  'com.android.support:customtabs:26.1.0'
    compile 'com.android.support:cardview-v7:26.1.0'
    //gson-2.7.jar in android_support_library folder
    compile(name: 'gson-2.7', ext: 'jar')
}
```

#### 3. Google Ads SDK 추가하기
Admob 광고를 프로젝트에 추가할 경우 Google 광고 지원을 포함시켜야 합니다. <br />
아래의 설명에 따라 의존성을 추가하실수 있습니다.
```groovy
    dependencies {
        compile 'com.google.android.gms:play-services-ads:17.2.0'
    }
```

>로컬 gms play가 의존하고 있는 aar파일만 추가하길 희망하신다면 gradle파일에서 이 구성을 무시할 수 있습니다. <br />
특히 프로젝트의 gms play-service 버전이 UPSDK 의존성과 다른 경우 상위 버전을 사용하세요.

#### 4. Facebook Ads SDK 추가하기
Facebook 삽입 광고(Interstitial Ad)를 프로젝트에 추가할 경우 Facebook 광고 지원을 프로젝트에 포함시켜야 합니다. <br />
아래의 설명에 따라 의존성을 추가하시기 바랍니다.

    dependencies {
        compile 'com.facebook.android:audience-network-sdk:4.99.1'
    }




### IV.  Proguard 설정 수정하기
프로젝트에서 `proguard`를 사용하는 경우, `proguard-project.txt`의 내용을 프로젝트에서 사용하는 <br />
`proguard`의 위치로 복사합니다.

### V. Demo 프로젝트
광고 SDK를 더 빠르고 쉽게 결할 수 있도록 간편한 [Demo 프로젝트](https://upsdk-korean.readthedocs.io/ko/master/Android/android08_demo.html)를 제공하고 있습니다.

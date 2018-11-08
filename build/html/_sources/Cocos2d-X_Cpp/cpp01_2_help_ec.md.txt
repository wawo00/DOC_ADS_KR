
## Eclipse

### I. UPSDK CppPlugin의 구조
Eclipse로 빌드된 프로젝트의 경우 UPSDK를 `*.jar`형식으로 프로젝트에 가져옵니다. UPSDK CppPlugin( [Download Android-CPPSDK ](http://doc.upltv.com/en/master/chapters/chapter09.html "download"))를 다운로드 하고 압축 해제를 합니다.

![ec-1-1](http://docc.upltv.com/uploads/201805/5afd3722e5ab6_5afd3722.png "ec-1-1")


- `Eclipse`: 이 디렉토리에는 Eclipse 프로젝트 액세스에 필요한 광고 의존성 라이브러리 파일이 주로 포함되어 있습니다.
- `cpp`: 이 디렉토리에는 주로 현재 Cocos2d-X cpp 프로젝트를 UPSDK 인터페이스 호출과 연동하기 위한 <br />
일부 *.cpp 원본 파일이 포함되어 있습니다.
- `Android Studio`: Eclipse를 사용하여 apk를 빌드하는 경우 이 디렉토리를 무시하시기 바랍니다.
- `proguard-project.txt`: 프로젝트에서 `proguard`를 사용하는 경우, 이 파일의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

### II. UPSDK 메인 패키지 가져오기

#### 1. UPSDK 파일 추가하기
UPSDK의 3.0.03 버전을 예시로 들겠습니다. `upsdk_ads` 디렉토리에는 `libs`, `res`및 `assets`의 세 개의 디렉토리가 있습니다.

디렉토리의 콘텐츠들을 Eclipse 메인 프로젝트 해당 디렉토리로 복사합니다. <br />
(참고:  libs 디렉토리가 없는 경우 src와 동일한 디렉토리를 생성합니다.) <br />
추가한 후의 구성은 아래와 같습니다.

![ec-2-1](http://docc.upltv.com/uploads/201805/5afd39e93c234_5afd39e9.png "ec-2-1")

> `UPAdsSdk_Cpp_3.0.03.jar`은 참조용 입니다.


#### 2. minSdkVersion 및 targetSdkVersion in `Manifest`에서 확인하기

UPSDK를 사용하려면 `minSdkVersion`이 15보다 크고, `targetSdkVersion`이 26보다 작아야 합니다. 이는 `build.gradle`에서 <br />
확인하시기 바랍니다.

```groovy
 < uses-sdk android:minSdkVersion="16" android:targetSdkVersion="24" />
```
#### 3. *.so 파일이 정렬되었는지 확인하기

폴더 이름 libs에는 세 가지 유형의 `libs`파일이 있습니다.

- armeabi
- armeabi-v7a
- x86

필요에 따라 libs 디렉토리에서 원하지 않는 CPU 아키텍처를 삭제하시기 바랍니다.

### III. 광고 네트워크 추가하기
#### 1. Google Ads SDK 추가

다음과 같이 **UPSDK의 파일 추가하기**를 참조하여 Google 광고를 아래와 같이 프로젝트에 추가합니다.

![ec-3-1](http://docc.upltv.com/uploads/201805/5afd45a3b7d61_5afd45a3.png "ec-3-1")

> 특히 프로젝트에 이미 다른 버전의 Google Play 서비스가 있는 경우 상위 버전을 사용하시기 바랍니다.

#### 2. 기타 광고 네트워크 추가하기

더 많은 수익을 위해 프로젝트에 가능한 한 많은 광고 라이브러리를 추가합니다.

`optional_ads`라는 이름의 폴더는 `*.jar`형식으로 존재하는 네트워크 의존성 파일을 포함하고 있습니다. **UPSDK의 파일 <br />
추가**를 참조하여 이 폴더를 프로젝트에 추가하고 AndroidManifest 파일의 내용을 프로젝트의 Manifest 파일에 <br />
복사합니다.

### IV. Android 지원 라이브러리 추가하기

광고 디스플레이는 `support` 라이브러리가 필요하기에 프로젝트로 가져옵니다. `android_support_library/libs`폴더에 <br />
해당 `xxx.jar` 파일이 있습니다. 다음과 같이 해당 파일을 `libs` 디렉토리에 추가합니다.

![ec-4-1](http://docc.upltv.com/uploads/201805/5afd4483c57c1_5afd4483.png "ec-4-1")


### V. AndroidManifest.xml 파일 수정
`AndroidManifest.xml` 파일의 콘텐츠를 프로젝트의 관련 위치로 복사합니다.


### Ⅵ. *.cpp 파일 추가하기


#### 1. 프로젝트에 파일 추가하기

UPSDK는 *cpp 소스 파일을 통해 네이티브 인터페이스(Native Interface)에 대한 크로스 플랫폼(Cross-Platform)을 <br />
구현하므로 `cpp/upltv`에 있는 모든 *.cpp 및 *.h 파일을 현재 프로젝트에 복사하시기 바랍니다.

Cocos2d-x 3.16 버전을 Classes 폴더에 복사할 수 있습니다. 버전 차이가 있는 경우 수정을 참조하시기 바랍니다. <br />
복사 후의 디렉토리는 아래와 같습니다.

![classes](http://docc.upltv.com/uploads/201804/5acac9170fddc_5acac917.png "classes")

>본문은 cocos2dx-3.16을 바탕으로 서술하고 있습니다. 디렉토리가 일치하지 않으면 담당자에게 문의하여 <br />
추가 지원을 받아보세요.

### 2. jni 디렉토리에서 android.mk 수정하기

프로젝트의 jni 디렉토리에 있는 android.mk 파일을 엽니다.

**UpltvAndroid.cpp,CocosUpLtv.cpp,UpltvBridge.cpp** 파일을 **LOCAL_SRC_FILES**로 복사합니다. <br />
Cocos2d-X 3.16을 예시로, 추가 방법은 다음과 같습니다. 기타 버전의 경우 해당되는 경로를 수정합니다.

```groovy
LOCAL_SRC_FILES := hellocpp/main.cpp \
                   ../../Classes/AppDelegate.cpp \
                   ../../Classes/upltv/UpltvAndroid.cpp \
                   ../../Classes/upltv/CocosUpLtv.cpp \
                   ../../Classes/upltv/UpltvBridge.cpp \
                   ../../Classes/HelloWorldScene.cpp
```

### Ⅶ. Proguard 설정 수정하기

프로젝트에서 `proguard`를 사용하는 경우, `proguard-project.txt`에서 파일의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

### Ⅷ.Demo 프로젝트
광고 SDK를 쉽고 빠르게 결합할 수 있도록 [Demo 프로젝트 ](https://github.com/AvidlyGit/AdSdkDemo-Studio "Demo project")를 제공합니다.

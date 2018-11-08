## Android Eclipse


### I. UPSDK LuaPlugin 구조
Eclipse로 빌드된 프로젝트의 경우 UPSDK를 `*.jar`형식으로 프로젝트에 가져옵니다. <br />
UPSDK LuaPlugin를 [이곳](https://upsdk-korean.readthedocs.io/ko/master/chapters/chapter09.html)에서 다운로드 하고 압축 해제를 합니다.


![ec-1-1](http://docc.upltv.com/uploads/201805/5afe9bd143673_5afe9bd1.png "ec-1-1")
- `Eclipse`: 이 디렉토리에는 Eclipse 프로젝트 액세스에 필요한 광고 의존성 라이브러리 파일이 주로 포함되어 있습니다.
- `Lua`: 이 디렉토리에는 주로 현재 Cocos2d-X Lua 프로젝트를 UPSDK 인터페이스 호출과 연동하기 위한 일부 <br />
*.lua 원본 파일이 포함되어 있습니다.
- `Android Studio`: Eclipse를 사용하여 apk를 빌드하는 경우 이 디렉토리를 무시하시기 바랍니다.
- `proguard-project.txt`: 프로젝트에서 `proguard`를 사용하는 경우, 이 파일의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

### II. UPSDK 메인 패키지 가져오기
#### 1. UPSDK 파일 추가하기
UPSDK v3.0.03을 예시로 들겠습니다. upsdk_ads 디렉토리에는 libs, res 및 assets의 세 개의 디렉토리가 있습니다. <br />
디렉토리의 콘텐츠들을 Eclipse 메인 프로젝트 해당 디렉토리로 복사합니다. <br />
> (참고:  libs 디렉토리가 없는 경우 src와 동일한 디렉토리를 생성합니다.)

> *.so로 된 파일 이외의 파일 중에서, 복사 후에 중복된 파일 명을 발견했다면 신중히 확인하고 실행하시기 바랍니다. <br />
이름이 중복된 파일이 UpAdsSdk 원본 파일인 경우에는 덮어쓰기를 합니다. 상기된 파일 명 중복이 아닌 경우, <br />
개발자 본인의 경험에 따라(예를 들어, res/values/ 디렉토리에 있는 파일명 변경) 조치를 취합니다. <br />
UPLTV의 기술 팀에 문의하여 문제 해결 및 지원 서비스를 받을 수도 있습니다.

![ec-2-1](http://docc.upltv.com/uploads/201805/5afe9cf8d18fd_5afe9cf8.png "ec-2-1")
> `UPAdsSdk_Lua_3.0.03.jar`은 참조용입니다.

#### 2. `Manifest`에서 minSdkVersion 및 targetSdkVersion 확인하기
UPSDK를 사용하려면 `minSdkVersion`이 15보다 크고, `targetSdkVersion`이 26보다 작아야 합니다. <br />
이는 build.gradle 에서 확인하시기 바랍니다.
```groovy
 < uses-sdk android:minSdkVersion="16" android:targetSdkVersion="25" />
```
#### 3. *.so 파일이 정렬되었는지 확인하기

`libs` 폴더에는 cpu 유형의 .so 파일의  3 가지 유형의 .so 파일이 있습니다.

- armeabi
- armeabi-v7a
- x86

필요에 따라 libs 디렉토리에서 원하지 않는 CPU 아키텍처를 삭제하시기 바랍니다.

### III. 광고 네트워크 추가하기
#### 1. Google Ads SDK 추가
다음과 같이 UPSDK의 파일 추가하기를 참조하여 Google 광고를 아래와 같이 프로젝트에 추가합니다.

![ec-3-1](http://docc.upltv.com/uploads/201805/5afea0b542f2f_5afea0b5.png "ec-3-1")
> 특히 프로젝트에 이미 다른 버전의 Google Play 서비스가 있는 경우 상위 버전을 사용하시기 바랍니다.

#### 2. 기타 광고 네트워크 추가하기
더 많은 수익을 위해 프로젝트에 가능한 한 많은 광고 라이브러리를 추가합니다. `optional_ads`라는 이름의 폴더는 <br />
`*.jar`형식으로 존재하는 네트워크 의존성 파일을 포함하고 있습니다. UPSDK의 파일 추가를 참조하여 이 폴더를 <br />
프로젝트에 추가하고 `AndroidManifest` 파일의 내용을 프로젝트의 `Manifest` 파일에 복사합니다.

### IV. Android 지원 라이브러리 추가하기
광고 디스플레이는 `support` 라이브러리가 필요하기에 프로젝트로 가져옵니다. `android_support_library/libs`폴더에 <br />
해당 `xxx.jar` 파일이 있습니다. 다음과 같이 해당 파일을 `libs` 디렉토리에 추가합니다.

![ec-4-1](http://docc.upltv.com/uploads/201805/5afea104016ef_5afea104.png "ec-4-1")

### V. AndroidManifest.xml 파일 수정하기

`AndroidManifest.xml` 파일의 콘텐츠를 프로젝트 내의 해당 위치에 복사합니다.

### Ⅵ. *.mk 파일 수정하기

현재 프로젝트 jni 디렉토리에 application.mk 파일을 열고 아래의 정보를 추가합니다.
```groovy
APP_SHORT_COMMANDS := true
NDK_TOOLCHAIN_VERSION=4.9
APP_PLATFORM=android-14
```

### Ⅶ. Proguard 설정 수정하기
만약 프로젝트에서 proguard를 사용하신다면 `proguard-project.txt`에서 파일의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

### Ⅷ.Demo 프로젝트
광고 SDK를 쉽고 빠르게 결합할 수 있도록 [Demo 프로젝트](https://github.com/AvidlyGit/AdSdkDemo-Studio "Demo工程")를 제공합니다.

## Eclipse

### I. UPSDK CppPlugin의 구조

Eclipse로 빌드된 프로젝트의 경우 UPSDK를 `*.jar`형식으로 프로젝트에 가져옵니다. UPSDK JavaScriptPlugin를 <br />
[이곳](http://doc.upltv.com/en/master/chapters/chapter09.html "download")에서 다운로드 하고 압축 해제를 합니다.

![](http://docc.upltv.com/uploads/201809/5b98f4a1c89a1_5b98f4a1.png)

- `Eclipse`: 이 디렉토리에는 일부 *.jar 및 리소스 파일이 포함되어 있습니다. Android 스튜디오를 사용하여  <br />
apk를 빌드하는 경우 이 디렉토리를 무시하시기 바랍니다.
- `js`: 이 디렉토리에는 현재 `Layabox Js` 프로젝트를 UPSDK 인터페이스로 연동하기 위한 일부 `*.js` 소스 파일들이 <br />
포함되어 있습니다.
- `Android Studio`: 이 디렉토리에는 Android 스튜디오에 필요한 광고 의존성 라이브러리 파일들이 포함되어 있습니다.
- `proguard-project.txt`: 프로젝트에서 `proguard`를 사용하는 경우, 파일의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

### II. UPSDK 메인 패키지 가져오기

#### 1. UPSDK 파일 추가하기

UPSDK의 3.0.05 버전을 예시로 들겠습니다. `upsdk_ads` 디렉토리에는 `libs`, `res` 및 `assets`의 세 개의 디렉토리가 있습니다. <br />
디렉토리의 콘텐츠들을 Eclipse 메인 프로젝트 해당 디렉토리로 복사합니다. <br />
(참고: libs 디렉토리가 없는 경우 src와 동일한 디렉토리를 생성합니다.)

추가한 후의 구성은 아래와 같습니다.

![](http://docc.upltv.com/uploads/201809/5b98f614c67d9_5b98f614.png)

> `UPAdsSdk_LayaJs_3.0.05.jar`은 참조용입니다.

#### 2. `Manifest`에서 minSdkVersion 및 targetSdkVersion 확인하기

UPSDK를 사용하려면 `minSdkVersion`이 15보다 크고, `targetSdkVersion`이 26보다 작아야 합니다. <br />
이는 `build.gradle`에서 확인하시기 바랍니다.

```groovy
 < uses-sdk android:minSdkVersion="16" android:targetSdkVersion="24" />
```
#### 3. `*.so`파일이 정렬되었는지 확인하기

폴더 이름 `libs`에는 세 가지 cpu유형의 .so 파일이 있습니다.

- armeabi
- armeabi-v7a
- x86

필요에 따라 libs 디렉토리에서 원하지 않는 CPU 아키텍처를 삭제하시기 바랍니다.

### III. 광고 네트워크 추가하기

#### 1. Google Ads SDK 추가

다음과 같이 **UPSDK의 파일 추가하기**를 참조하여 Google 광고를 아래와 같이 프로젝트에 추가합니다.

![ec-3-1](http://docc.upltv.com/uploads/201809/5b98f679af237_5b98f679.png "ec-3-1")

> 프로젝트에 이미 다른 버전의 Google Play 서비스가 있는 경우 상위 버전을 사용하시기 바랍니다.

#### 2. 기타 광고 네트워크 추가하기

더 많은 수익을 위해 프로젝트에 가능한 한 많은 광고 라이브러리를 추가합니다. `optional_ads`라는 이름의 폴더는 <br />
`*.jar`형식으로 존재하는 네트워크 의존성 파일을 포함하고 있습니다. **UPSDK의 파일 추가**를 참조하여 이 폴더를 <br />
프로젝트에 추가하고 AndroidManifest 파일의 내용을 프로젝트의 Manifest 파일에 복사합니다.

### IV. Android 지원 라이브러리 추가하기

광고 디스플레이는 `support` 라이브러리가 필요하기에 프로젝트로 가져옵니다. `android_support_library/libs` 폴더에 <br />
해당 `xxx.jar` 파일이 있습니다. 다음과 같이 해당 파일을 `libs` 디렉토리에 추가합니다.

![ec-4-1](http://docc.upltv.com/uploads/201809/5b98f6edd1b61_5b98f6ed.png)

### V. AndroidManifest.xml 파일 수정

`AndroidManifest.xml`파일의 콘텐츠를 프로젝트의 관련 위치로 복사합니다.


### Ⅵ. `*.js` 파일 추가하기

#### 1. 프로젝트에 파일 추가하기

UPSDK는 `*js` 소스 파일을 통해 네이티브 인터페이스(Native Interface)에 대한 크로스 플랫폼(Cross-Platform)을 <br />
구현하므로 `js/src/upltv`에 있는 모든 `*.js` 파일을 현재 프로젝트에 복사하시기 바랍니다.

- Layabox 프로젝트의 `src` 폴더에 `upltv` 를 복사하고, `.js` 파일만 남겨둡니다.

- 아래와 같이 Layabox 프로젝트의 bin폴더 안에 있는 index.html에 `UPLTV.js`와 `UPLTVIos.js`의 참조를 추가합니다.

![](http://docc.upltv.com/uploads/201809/5b98f2c8af661_5b98f2c8.png)

### Ⅶ. Proguard 설정 수정하기

만약 프로젝트에서 `proguard`를 사용하신다면 `proguard-project.txt`에서 이 파일의 내용을 프로젝트에서 <br />
사용하는 `proguard` 위치로 복사합니다.

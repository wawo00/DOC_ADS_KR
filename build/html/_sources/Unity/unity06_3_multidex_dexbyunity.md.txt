## Unity

Unity 5.5 및 상위 버전은 Multidex를 지원하며 Android Studio 프로젝트로 내보낼 필요가 없으며 <br />
Unity IDE에서 직접 sunbcontracting 설정을 완료할 수 있습니다.

### I. `Gradle build system` 열기

- `Unity Editor`에서 `Build Settings` 열기(File -> Build Settings)
- 리스트에서 `Android` 선택
- `Build System`을 `Gradle(new)`로 설정

Unity IDE의 서로 다른 버전에서의 빌드 설정이 약간 다를 수 있습니다.

![666](http://docc.upltv.com/uploads/201807/5b39e51a967db_5b39e51a.jpeg "666")

### II. `Gradle settings` 수정하기
##### 1. Unity 2017.2 및 상위 버전에서 다음과 같이 `Player Settings`을 열고  `Custom Gradle Template` 확인란을 <br />
선택할 수 있습니다. 다른 버전의 경우 `mainTemplate.gradle`파일을 (Unity 설치 목록에서 mainTemplate 검색) <br />
`Assets/Plugins/Android/mainTemplate.gradle`에 복사해야 합니다.

![777](http://docc.upltv.com/uploads/201807/5b39ec4b74539_5b39ec4b.jpeg "777")

##### 2. `defaultConfig`에서 `multiDexEnabled true`를 추가하기

    ```groovy
    defaultConfig {
        targetSdkVersion **TARGETSDKVERSION**
        applicationId '**APPLICATIONID**'
        multiDexEnabled true
        ndk {
                abiFilters **ABIFILTERS**
            }
    }
    ```

##### 3. independencies에 com.android.support:multidex:1.0.1를 추가하기

`com.android.support:multidex:1.0.1`

```groovy
dependencies {
    compile 'com.android.support:multidex:1.0.1'
    compile fileTree(dir: 'libs', include: ['*.jar'])
}
```

##### 4. minifyEnabled와 useProguard setting 제거하기
`shrinking/minification is not supported with Multidex` 에러가 있다면,  <br />
useProguard 매개 변수 및 minifyEnabled 매개 변수를 제거하시기 바랍니다.

### III. Multidex 초기화
AndroidManifest.xml이 `Assets/Plugins/Android/`에 없는 경우, <br />
Unity 설치 목록에서 default AndroidManifest.xml을 이 디렉토리로 복사합니다.

`AndroidManifest.xml`의 애플리케이션 태그에 `android:name`으로 설정된 MultiDexApplication <br />
또는 MultiDexApplication 하위 클래스가 없는 경우 `android.support.multidex.MultiDexApplication`을 <br />
 `android:name` 으로 추가하시기 바랍니다.

```xml
 <?xml version="1.0" encoding="utf-8"?>
 <manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.myapp">
     <application
            android:name="android.support.multidex.MultiDexApplication" >
        ...
     </application>
 </manifest>
```

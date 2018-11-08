## Android Studio

Unity 5.5부터 Gradle 빌드 시스템을 지원합니다. 65535 초과 에러의 경우 프로젝트를 Android 스튜디어로 내보내고 gradle 을 이용해 이 문제를 해결할 수 있습니다.


### 1. Android Studio로 프로젝트 불러오기

![3333](http://docc.upltv.com/uploads/201807/5b39cc6bd83bb_5b39cc6b.png "3333")


### 2. Android Studio 열기
Android Studio 2.2.3 및 상위 버전이 설치되어 있어야 합니다.

![4444](http://docc.upltv.com/uploads/201807/5b39ccc80a994_5b39ccc8.png "4444")

### 3. build.gradle 구성
Android Studio 2.2.3 및 상위 버전이 설치되어 있어야 합니다.

![555](http://docc.upltv.com/uploads/201807/5b39cd2136c17_5b39cd21.png "555")

build.gradle 파일에 subcontracting 설정 추가:

```groovy
android {
    defaultConfig {
        ...
        minSdkVersion 16
        targetSdkVersion 26
        multiDexEnabled true
    }
    ...
}
dependencies {
  compile 'com.android.support:multidex:1.0.1'
}
```

**AndroidManifest.xml** 에 아래 사항을 추가합니다.：
```groovy
 <application
            android:name="android.support.multidex.MultiDexApplication" >
        ...
 </application>
```

### 4. OutOfMemory 해결
다음 구성을 추가하여 프로젝트를 실행할 때 생기는 gradle.build내의 OutOfMemory 문제를 해결합니다.

```groovy
android {
    defaultConfig {
        ....
    }
    dexOptions {
        javaMaxHeapSize "4g"
    }
}
```

### 5. Proguard 설정 수정
`Build-in class shrinker and multidex are not supported yet` 에러 메시지는 대부분의 경우 <br />
 useProguard 필드에 의해 발생합니다. minifyEnabled 대신 useProguard를 사용해 보시기 바랍니다.

```groovy
android {
    buildTypes {
        release {
            minifyEnabled true
        }
    }
}
```
Proguard 설정이 필요치 않을 시에는, minifyEnabled를 제거하거나 값을 false로 설정하시기 바랍니다.

## 65535 제한 문제 해결

메소드 수가 65535의 제한을 초과하면 apk가 생성될 때 다음과 같은 에러가 발생합니다.

![222](http://docc.upltv.com/uploads/201807/5b39ca2058b2a_5b39ca20.png "222")

> `trouble writing output: Too many method references to fit in one dex file: 67449; max is 65536.`

이러한 문제를 해결하기 위해 Unity에서 5.5 버전부터 Multidex를 지원하고 있습니다.

### 1. [Android Studio ](./unity06_2_multidex_dexbyas.html "Android Studio分包")
Android Studio 2.2.3 및 상위 버전에서는 프로젝트에서 apk 패키징을 gradle을 통해 완료시킬 수 있습니다.

### 2. [UnityIDE ](./unity06_3_multidex_dexbyunity.html "UnityIDE分包")
필수 파일을 간단하게 복사하고, 세팅을 초기화 시켜 간단하게 Subcontracting 하실 수 있습니다.

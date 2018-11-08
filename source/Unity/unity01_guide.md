## Unity Plugin 가이드

C#으로 설계된 UPLTV AD SDK(Android,iOS 플랫폼 및 의존성 라이브러리)를 포함하는 <br />
UPSDK Unity Plugin은 native API 호출을 제공하며, 삽입 광고(Interstitial Ad), 삽입 광고(Interstitial Ad) <br />
및 배너 광고(Banner Ad)와 같은 비즈니스 기능을 구현합니다.

UPLTV는 아래와 같은 플랫폼 및 버전을 지원합니다.

- `Android`에서는 Unity 5.0.0 및 상위 버전만 지원합니다.
- `iOS`에서는 Unity 5.1.0 및 상위 버전만 지원합니다.

> 하위 버전에 대한 지원이 필요하시다면 담당자에게 문의하여 추가 지원을 받아보세요.

UPSDK Unity Plugin에는 Android 및 iOS 플랫폼에 필요한 지원 요소가 포함되어 있습니다. <br />
iOS SDK의 용량은 Android 버전과 차이가 크므로, 세 가지 액세스 방법이 제공됩니다. <br />
상황에 따라 적절한 방법을 선택하시기 바랍니다.

##### 1. Android 플랫폼 전용
> Android 플랫폼용으로만 앱을 출시하는 경우 Android 플랫폼 Plugin만 선택하는 것이 좋습니다. <br />
Plugin의 크기(<10M)는 iOS(540M)에 비해 매우 가볍습니다. <br />

##### 2. iOS 플랫폼 전용
>iOS 플랫폼용으로만 앱을 출시하는 경우 iOS 플랫폼 Plugin만 선택하는 것이 좋습니다. <br />
Plugin 패키지가 크므로(UPSDK의 iOS 지원 및 타사 라이브러리 포함) 다운로드할 때 네트워크 연결을 <br />
안정적으로 유지하시기바랍니다. <br />
> iOS Plugin 패키지가 Android보다 훨씬 크기 때문에, 더 이상 iOS Plugin 패키지를 개별로 제공하지 않습니다. <br />

##### 3. Android and iOS
> Android 및 iOS 플랫폼을 모두 고려해 설계된 앱인 경우, UPSDK Unity Plugin을 권장합니다. <br />
이 패키지(550M)에는 UPSDK 및 해당 타사 라이브러리의 Android 및 iOS 지원이 포함됩니다. <br />
(다운로드 시 안정된 네트워크 상태를 유지하시기 바랍니다).

#### [UPSDK Unity Plugin 다운로드](https://upsdk-korean.readthedocs.io/ko/master/chapters/chapter09.html)

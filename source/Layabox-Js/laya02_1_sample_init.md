## SDK 초기화

이 부분에서는 Xcode를 사용한 샘플만 설명드리고 있습니다. <br />
다른 개발 툴을 사용하고 계신 분들께 불편을 끼쳐 드려 죄송합니다.

### UPSDK 초기화
```javascript
/*
* 다른 API 인터페이스의 SDK를 사용하기 전에 SDK 초기화를 완료합니다.
* @param zone product distribution area, 0: 중국을 제외한 해외, 1: 중국, 2: IP에 의해 자동 설정
*/
intSdk : function(zone, callback)
```

샘플：

```javascript
var initSdkButton = this.createButton(x, y, "initSdk");
initSdkButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.intSdk(0);
    }
}, this);
```

###  setCustomerId()

Android 전용으로, 중국 시장에만 릴리즈 하실 계획이라면, androidid 를 설정하여 통계의 정확도를 확실하게 합니다.

```asp

// 초기화하기 전에 이 메소드를 호출합니다.
// 3004(하위버전 5) 및 상위 버전에서만 이 메소드를 지원하고 있습니다.
// @param androidid

upltv.setCustomerId(androidid)
```

###  onApplicationFocus() 추가하기

Android 플랫폼에 대해, 게임의 포그라운드/백그라운드 전환 시, 아래의 두 UPSDK 인터페이스를 호출하셔서 <br />
UPSDK가 올바르게 응답하게 하실 것을 권장합니다.

#### 게임의 포그라운드 상태에서 메소드 호출하기

```java
UPAdsSdk.onApplicationResume();
```
#### 게임의 백그라운드 상태에서 메소드 호출하기

```java
UPAdsSdk.onApplicationPause();
```

## SDK 초기화

이 부분에서는 Xcode를 사용한 샘플만 설명드리고 있습니다. <br />
다른 개발 툴을 사용하고 계신 분들께 불편을 끼쳐 드려 죄송합니다.

UPLTV SDK를 통해 간단히 초기화하실 수 있습니다.

### 아래의 참조를 프로젝트에 추가합니다.
`UPLTV.js`와 `UPLTVIos.js`를 `src->upltv`디렉토리에 추가하고, `project.json`파일에 추가합니다.

```javascript
"jsList" : [
        "src/resource.js",
        "src/app.js",
        "src/upltv/UPLTV.js",
        "src/upltv/UPLTVIos.js"
]
```
### UPSDK 초기화

```javascript
/**
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
####  setCustomerId()

Android 전용으로, 중국 시장에만 릴리즈 하실 계획이라면, androidid 를 설정하여 통계의 정확도를 확실시해야 합니다.

```asp
/*
 * 초기화하기 전에 이 메소드를 호출합니다.
 * 3004(하위버전 5) 및 상위 버전에서만 이 메소드를 지원하고 있습니다.
 * @param androidid
 */
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

샘플:

보통 Cocos2d-X 엔진으로 게임을 활성화 할 때, `org.cocos2dx.lib.Cocos2dxActivity`의 하위 문서를 이어 받습니다. <br />
Demo 프로젝트 안의 이 클래스는 AppActivity 입니다.

```java
    @Override
    protected void onResume() {
        super.onResume();
        UPAdsSdk.onApplicationResume();
    }

    @Override
    protected void onPause() {
        super.onPause();
        UPAdsSdk.onApplicationPause();
    }
```

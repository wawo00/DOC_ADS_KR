## SDK 초기화

이 부분에서는 Xcode를 사용한 예시만 설명드리고 있습니다. <br />
다른 개발 툴을 사용하고 계신 분들께 불편을 끼쳐 드려 죄송합니다.


UPLTV SDK를 통해 간단히 초기화하실 수 있습니다.


아래의 참조를 프로젝트에 추가합니다.
```lua
local upltv = require "src.app.views.UPLTV"
```
###  SDK 초기화
```lua
-- parameter zone-- 제품 퍼블리싱 지역, 0: 중국을 제외한 해외, 1: 중국,  2: IP에 의해 자동 설정
-- SDK 초기화 이후 콜백 인터페이스를 완료합니다. 콜백 인터페이스는 Boolean 매개변수 콜백(Boolean)을 포함하고 있으며, True는 성공을 의미합니다.
upltv:initSDK(zone, ...)
```
예시：
```lua
local btn = createBtnAt(self, left, top, fontsize, "init")
btn:addTouchEventListener(function(sender, eventType)
    if (2 == eventType) then
       local zone = 0
       local callback = function(r)
            if r == true then
               print("===> initSDK success")
           else
               print("===> initSDK fail")
           end
        end
        upltv:initSDK(zone, callback)
    end
end)
```

### setCustomerId
Android 플랫폼에서만 지원합니다. GP가 아닌 패키지일 경우, 부가적인 통계적 오류를 방지하기 위해 <br />
androidid로 전달할 수 있습니다.

```lua
-- 초기화하기 전에 이 메소드를 호출합니다.
-- 3004(하위버전 5) 및 상위 버전에서만 이 메소드를 지원하고 있습니다.
-- @param androidid
upltv:setCustomerId(androidid)
```

### onApplicationFocus() 추가하기
안드로이드 플랫폼에 대해, 현재 게임의 포그라운드/백그라운드 전환 시, 아래의 두 개의 UPSDK의 인터페이스를 <br />
호출 해야 합니다. UPSDK가 올바르게 반응하여 불필요한 에러를 피할 수 있습니다.

#### 게임이 포그라운드로 돌아올 때 API를 호출합니다.
```java
UPAdsSdk.onApplicationResume();
```
#### 게임이 백그라운드로 돌아올 때 API를 호출합니다.
```java
UPAdsSdk.onApplicationPause();
```

예시：

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

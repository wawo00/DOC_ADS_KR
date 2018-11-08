## 삽입 광고(Interstitial Ad) API
### 1. 삽입 광고 로드 콜백 설정하기
현재 삽입 광고를 수신하기 위해 성공 또는 실패 결과를 로드합니다. 이 인터페이스는 콜백이 발생하면 자동으로 <br />
릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.

```lua
-- 삽입 광고의 준비 여부는 매개변수에 의해 판단되고, Bool 결과를 동시에 반환합니다. True는 광고가 디스플레이 준비가 되었다는 것을 의미하고, False는 광고가 실행되지 않고, 여전히 요청상태에 있다는 것을 의미합니다.
-- cpPlaceId：placementid
-- callback: true 혹은 false 를 콜백합니다.
upltv:isInterstitialReadyAsyn(cpPlaceId, callback)
```
샘플：
```lua
local btnInterstitialShow = createBtnAt(self, left, top, fontsize, "iLReadyAsyn")
btnInterstitialShow:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:isInterstitialReadyAsyn(ilPlaceId, function(r)
            if r == true then
                print("=====> Interstitial is ready")
            else
                print("=====> Interstitial is not ready")
            end
        end)
    end
end)
```
### 2. 삽입 광고 실행 콜백 설정하기

콜백의 실행을 수신할 수 있는 삽입 광고의 디스플레이를 위해 콜백 인터페이스를 설정합니다. <br />
삽입 광고는 콜백 인터페이스에 대한 참조가 저장되고 릴리즈되지 않음을 나타냅니다.
```lua
-- 재생 시간 동안 클릭 및 닫기 등의 이벤트 콜백을 수신하는 데 사용되는 삽입 광고의 디스플레이를 위한 콜백 인터페이스를 설정합니다.
-- plugin은 콜백 인터페이스가 저장되고 릴리즈되지 않는다는 참조를 나타냅니다.
-- 콜백 이벤트 유형: 실행, 클릭, 닫기
-- @param cpPlaceid placementid, showCall(유형, cpPlaceId) 호출
upltv:setInterstitialShowCallback(cpPlaceId, showCall)
```
샘플：
```lua
local btnInterstitialShowCall = createBtnAt(self, left, top, fontsize, "iLShowCall")
btnInterstitialShowCall:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local function ilShowCall(type, cpadid)
            local event = "unkown"
            if type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_SHOW then
                event = "Did_Show"
            elseif type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_CLICK then
                event = "Did_Click"
            elseif type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_CLOSE then
                event = "Did_Close"
            end
            print("=====> Interstitial ShowCall Event:" .. event ..", at :" .. cpadid)
        end
        upltv:setInterstitialShowCallback(ilPlaceId, ilShowCall)
    end
end)
```
### 3. 삽입 광고 준비 여부 확인하기

삽입 광고의 준비 여부는 매개변수에 의해 판단되고, Bool결과를 동시에 반환합니다. True는 광고가 디스플레이 준비가 <br />
되었다는 것을 의미하고, False는 광고가 실행되지 않고, 여전히 요청상태에 있다는 것을 의미합니다.
```lua
upltv:isInterstitialReady(cpPlaceId)
```
샘플：
```lua
local btnInterstitialShow = createBtnAt(self, left, top, fontsize, "iLReadyAsyn")
btnInterstitialShow:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:isInterstitialReadyAsyn(ilPlaceId, function(r)
            if r == true then
                print("=====> Interstitial is ready")
            else
                print("=====> Interstitial is not ready")
            end
        end)
    end
end)
```
### 4. 삽입 광고 실행하기
매개변수 `Placement ID`에 따른 삽입 광고가 실행됩니다.
```lua
upltv:showInterstitialAd(cpPlaceId)
```
샘플：
```lua
local btnInterstitialShow = createBtnAt(self, left, top, fontsize, "iLShow")
btnInterstitialShow:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        if upltv:isInterstitialReady(ilPlaceId) == true then
            print("=====> Interstitial is ready")
            upltv:showInterstitialAd(ilPlaceId)
        else
            print("=====> Interstitial is not ready")
        end
    end
end)
```
### 5. 삽입 광고의 디버그 페이지

개발자가 광고의 사용 및 로딩 상태를 볼 수 있도록 하기 위해, UPLTV는 스크린 광고에 대한 디버그 페이지를 제공합니다. 광고의 매개변수 및 로딩 상태 구조를 확인하실 수 있습니다.
```lua
upltv:showInterstitialDebugUI()
```
샘플：
```lua
local btnshowInterActivity = createBtnAt(self, left, top, fontsize, "iLDebugUI")
btnshowInterActivity:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:showInterstitialDebugUI()
    end
end)
```

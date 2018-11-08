## 배너 광고(Banner Ad)
배너 광고(Banner Ad)는 상단 배너와 하단 배너로 구분되며, LuaPlugin은 디스플레이, 숨기기, 제거 및 이벤트 콜백과 같은 <br />
인터페이스를 제공하여 배너 광고의 구현을 더욱 단순화 합니다.

### 1. 배너 콜백
배너 광고는 배너 광고 및 클릭, 이벤트 콜백 인터페이스의 디스플레이를 설정해야 합니다. 콜백 인터페이스는 <br />
Plug-in에 의해 내부적으로 저장되므로 여러 번 설정할 필요가 없습니다. <br />
upltv:removeBannerAdAt(cpPlaceId)라는 호출만 삭제됩니다.

```lua
-- 배너 광고의 디스플레이 콜백 인터페이스를 설정하고 , 콜백 인터페이스는 저장되며 upltv:removeBannerAdAt(cpPlaceId) 호출만 삭제됩니다.
-- cpPlaceId：banner placementID
-- showCall: 클릭하여 콜백을 스킵하거나 제거합니다.
-- callback parameter: 이벤트 유형, placementID,  showCall(유형, cpPlaceId)
 upltv:setBannerShowCallback(cpPlaceId, showCall)
```
예시：
```lua
local btnsetbannercall = createBtnAt(self, left, top, fontsize, "bannerCall")
btnsetbannercall:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local function bannerShowCall(type, cpadid)
            local event = "unkown"
            if type == upltv.AdEventType.BANNER_EVENT_DID_SHOW then
                event = "Did_Show"
            elseif type == upltv.AdEventType.BANNER_EVENT_DID_CLICK then
                event = "Did_Click"
            elseif type == upltv.AdEventType.BANNER_EVENT_DID_REMOVED then
                event = "Did_Removed"
            end
            print("=====> banner ShowCall Event:" .. event ..", at :" .. cpadid)
        end
        upltv:setBannerShowCallback("BannerAd",bannerShowCall)
        upltv:setBannerShowCallback("banner_bbb",bannerShowCall)
    end
end)
```

### 2. 상단 배너 광고 실행하기
매개변수 `Placement ID`에 따른 스크린의 상단에 배너가 실행됩니다.
```lua
-- According to  'cpPlaceId' show banner ads at the top of the current screen
upltv:showBannerAdAtTop(cpPlaceId)
```

예시：
```lua
-- 상단 배너 광고를 실행합니다.
top = top - disht
local btnshowtopbanner = createBtnAt(self, left, top, fontsize, "topBanner")
btnshowtopbanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:showBannerAdAtTop("BannerAd")
    end
end)
```

**상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.**
```lua
-- 상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.
-- @param padding: 상단 배너의 상쇄 값, 예를 들어 32를 입력하면 32 픽셀만큼 아래로 이동합니다.
-- 이 기능은 Android 플랫폼에서는 지원되지 않으며, 3003 버전부터 지원하고 있습니다.
upltv:setTopBannerPadingForIphonex(padding)
```
예시：
```lua
top = top - disht
local btnshowtopbanner = createBtnAt(self, left, top, fontsize, "topBanner")
btnshowtopbanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:setTopBannerPadingForIphonex(50)
    end
end)
```

### 3. 상단 배너 광고 숨기기

```lua
upltv:hideBannerAdAtTop()
```
예시：
```lua
local btnhideallbanner = createBtnAt(self, left, top, fontsize, "hideAll")
btnhideallbanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:hideBannerAdAtTop()
    end
end)
```
### 4. 하단 배너 광고 실행하기
매개변수 `Placement ID`에 따른 스크린의 하단 배너가 실행됩니다.
```lua
upltv:showBannerAdAtBottom(cpPlaceId)
```
예시：
```lua
local btnshowbottombanner = createBtnAt(self, left, top,fontsize, "bottomBanner")
btnshowbottombanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:showBannerAdAtBottom("banner_bbb")
    end
end)
```
### 5. 하단 배너 광고 숨기기

```lua
upltv:hideBannerAdAtBottom()
```
예시：
```lua
local btnhideallbanner = createBtnAt(self, left, top, fontsize, "hideAll")
btnhideallbanner:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:hideBannerAdAtBottom()
    end
end)
```
### 6. 배너 광고 삭제하기
광고 placement ID에 따른 배너를 제거할 수 있습니다.

```lua
upltv:removeBannerAdAt(cpPlaceId)
```
예시：
```lua
local btnremoveallbanner = createBtnAt(self, left, top, fontsize, "removeAll")
btnremoveallbanner:addTouchEventListener(function(sender,eventType)
    if  (2 == eventType) then
        upltv:removeBannerAdAt("banner_bbb")
        upltv:removeBannerAdAt("BannerAd")
    end
end)
```

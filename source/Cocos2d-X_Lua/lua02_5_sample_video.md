## 리워드 동영상 광고(Rewarded Video Ad)

#### 1. 리워드 동영상 광고 콜백 로드하기

이 API는 현재 진행 중인 리워드 동영상 광고의 로딩 결과를 수신하는 데 사용됩니다. 이 인터페이스가 호출되면 <br />
내부가 자동으로 릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.

```lua
-- @param loadsuccess 동영상이 loaded loadsuccess(cpadid, msg) 됐을 때, 콜백을 활성화 합니다.
-- @param failCall    동영상이 loaded failed，locadfail(cpadid, msg) 됐을 때, 콜백을 활성화 합니다.
upltv:setRewardVideoLoadCallback(loadsuccess, locadfail)
```
샘플：
```lua
local btnshowvideoActivity = createBtnAt(self, left, top, fontsize, "rdLoadCall")
btnshowvideoActivity:addTouchEventListener(function(sender, eventType)
    if  (2== eventType) then
        upltv:setRewardVideoLoadCallback(function(cpadid,msg)
            print("=====>RewardVideoLoadCallback success at " .. cpadid)         
        end,
        function(cpadid, msg)
            print("=====> RewardVideoLoadCallback fail at " .. cpadid .. " because of " .. msg)
        end)
    end
end)
```
#### 2. 리워드 동영상 광고의 실행 콜백
리워드 동영상 광고의 이벤트(클릭, 닫기, 보상 등) 콜백을 수신하도록 콜백 인터페이스를 설정합니다. <br />
리워드 동영상은 콜백 인터페이스에 대한 참조가 내부적으로 저장되고 릴리스되지 않으므로 한 번만 설정하면 됩니다.


```lua
-- 클릭, 닫기, 보상과 같은 이벤트 콜백을 모니터 하는 리워드 동영상 디스플레이 콜백 인터페이스를 설정합니다.
-- 참조는 내부적으로 저장되고, 릴리즈 되지 않습니다.
-- 콜백 이벤트 유형: 디스플레이, 클릭, 닫기, 보상 성공, 보상 실패
-- @param：showCall ,showCall(type, cpadid)
upltv:setRewardVideoShowCallback(showCall)
```

샘플：
```lua
local btnshowvideoActivity = createBtnAt(self, left, top, fontsize, "rdShowCall")
btnshowvideoActivity:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local function rewardShowCall(type, cpadid)
            local event = "unkown"
            if type == upltv.AdEventType.VIDEO_EVENT_DID_SHOW then
                event = "Did_Show"
            elseif type == upltv.AdEventType.VIDEO_EVENT_DID_CLICK then
                event = "Did_Click"
            elseif type == upltv.AdEventType.VIDEO_EVENT_DID_CLOSE then
                event = "Did_Close"
            elseif type == upltv.AdEventType.VIDEO_EVENT_DID_GIVEN_REWARD then
                event = "Did_Given_Reward"
            elseif type == upltv.AdEventType.VIDEO_EVENT_DID_ABANDON_REWARD then
                event = "Did_Abandon_Reward"
            end
            print("=====> RewardVideo show call Event:" .. event ..", at :" .. cpadid)
        end
        upltv:setRewardVideoShowCallback(rewardShowCall)
    end
end)
```
#### 3. 리워드 동영상 광고 준비 여부 확인하기

Boolean 결과를 동시에 리턴합니다.  True는 광고가 디스플레이 준비가 되었다는 것을 의미하고, <br />
False는 광고가 실행되지 않고, 여전히 요청상태에 있다는 것을 의미합니다.

  ```lua

upltv:isRewardReady()
```

샘플：
```lua
local btnloadvideo = createBtnAt(self, left, top, fontsize, "rdReady")
btnloadvideo:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local r = upltv:isRewardReady()
        if (r==true) then
            print("=====> btn up isRewardReady: true")
        else
            print("=====> btn up isRewardReady: false")
        end
    end
end)
```
#### 4. 리워드 동영상 광고 실행하기

리워드 동영상 광고를 실행 할 때, cpPlaceld를 업로드 해야합니다. <br />
cpPlaceld는 비즈니스 관리와 같이 수익 소스를 구분하기 위한 매개변수입니다.

```lua
upltv:showRewardVideo(cpPlaceId)
```
샘플：
```lua
local btnloadvideo = createBtnAt(self, left, top, fontsize, "rdShow")
btnloadvideo:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local r = upltv:isRewardReady()
        if (r==true) then
            upltv:showRewardVideo("show_reward")
            print("=====> btn up isRewardReady: true")
        else
            print("=====> btn up isRewardReady: false")
        end
    end
end)
```
#### 5. 리워드 동영상 광고의 디버그 페이지
개발자가 광고의 사용 및 로딩 상태를 볼 수 있도록 하기 위해, UPLTV는 스크린 광고에 대한 디버그 페이지를 제공합니다. 광고의 매개변수 및 로딩 상태 구조를 확인하실 수 있습니다.

```lua
upltv:showRewardDebugUI()
```
샘플：
```lua
local btnshowvideoActivity = createBtnAt(self, left, top, fontsize, "rdDebugUi")
btnshowvideoActivity:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:showRewardDebugUI()
    end
end)
```

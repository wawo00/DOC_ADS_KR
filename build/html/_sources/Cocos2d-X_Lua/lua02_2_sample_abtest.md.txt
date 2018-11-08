## A/B 테스트

### 1. A/B 테스트 초기화 및 데이터 가져오기

SDK를 초기화한 후에 A/B테스트를 진행하시고, A/B 테스트 초기화를 완료하려면 이 메소드를 호출합니다.
```lua
-- call this method after initializing sdk
-- @param gameAccountId          string 유형, 게임 유저의 id (필수사항)
-- @param completeTask           bool 유형, 게임에서 튜토리얼 완수 여부
-- @param isPaid                 number 유형, 결제 유저 판단, 0이면 비결제 유저
-- @param promotionChannelName   string 유형, 광고 채널, 없으면  “ ” 전송 가능
-- @param gender                 string 유형, 성별 "M", "F", 모르면 “ “ 전송가능
-- @param age                    number 유형, 연령, -1은 unknown
-- @param tags                   String array 유형, 태그, 없으면 null 전송 가능
initAbtConfigJson : function(gameAccountId, isCompleteTask, isPaid, promotionChannelName, gender, age, tags)
```
예시：
```lua
local btninitAbtConfigJson = createBtnAt(self, left, top, fontsize, "initABtest")
btninitAbtConfigJson:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        upltv:initAbtConfigJson("u89731", true, 0, "Facebook", "M", -1, {"This is the first element.", "The second one.", "The last one."})
    end
end)
```

### 2. A/B 테스트 결과 얻기

A/B 테스트를 초기화한 후 이 메소드를 통해 결과를 얻을 수 있습니다.
```lua
-- 가져온 결과가 올바른지 확인하기 위해 initAbtConfigJson() 콜타임을 2초 이상 딜레이 하기를 권장합니다.
--  cpPlaceId : string 유형
upltv:getAbtConfig(cpPlaceId)
```
예시：
```lua
local btngetAbtConfigJson = createBtnAt(self, left, top, fontsize, "getAbtConfig")
btngetAbtConfigJson:addTouchEventListener(function(sender, eventType)
    if  (2 == eventType) then
        local r = upltv:getAbtConfig("pass")
        print("=====> getAbtConfig r:" .. type(r))
    end
end)
```

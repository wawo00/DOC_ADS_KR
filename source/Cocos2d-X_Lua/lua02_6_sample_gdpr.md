## GDPR

`GDPR(The General Data Protection Regulation)`은 2018년 5월 25일부터 시행된 EU(유럽연합)의 개인정보보호 법령이며, <br />
`UPSDK`는 아래와 같은 솔루션을 제공함으로써 `GDPR`을 준수하고 있습니다. EU 지역에서 서비스 하실 때에 안심하고 <br />
 사용하실 수 있습니다.

GDPR을 따른 UPSDK 버전은 `3.0.03`부터 적용되어 있습니다.

### GDPR 샘플
#### 맞춤형 구현

서비스를 최상의 상태로 구현하기 위해, 각 사의 게임 화면의 스타일에 따라 GDPR 권한 허가 창을 설정합니다. <br />
맞춤형 구현을 하셨다면, GDPR 권한 허가 결과를 UPSDK에 알려주시고 초기화를 진행합니다.


샘플：
```csharp
local function yourOwnGDPRCallback(result)

    -- result:  “true”는 게임 유저가 권한 승인했다는 것이고, false는 거부했다는 것을 의미합니다.

    if result=="true" then
        upltv:updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted)
    else
        upltv:updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusDefined)
    end

    -- UPSDK 초기화하기 전에 updateAccessPrivacyInfoStatus()를 호출합니다.

    upltv:initSDK(0)
end

local function europeanUnionUserCallBack(result)
    print("=====> lua mEuropeanUnionUserCallBack result: " .. result)
    -- result: "true" means users in the EU region

    if result=="true" then

        -- callYourOwnGDPRDialog()는 호출 메소드입니다.
        -- yourOwnGDPRCallback은 결과를 받는 콜백 입니다.
        -- Fake code
        callYourOwnGDPRDialog(yourOwnGDPRCallback)
    else
        upltv:initSDK(0)
    end
end

-- GDPR
local function checkGDPR()
    local e = upltv:getAccessPrivacyInfoStatus()
    print("=====> lua getAccessPrivacyInfoStatus status: " .. e)

    -- 권한 승인 요청을 하지 않았다면
    if e==upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusUnkown then

        -- 게임 유저가 EU 지역에 있는지 확인합니다.
        upltv:isEuropeanUnionUser(europeanUnionUserCallBack)
    else
        upltv:initSDK(0)
    end
end
```

#### 빠른 구현
UPSDK에서 제공하는 표준 허가 프로세스를 사용하신다면 아래와 같은 코드를 사용합니다.

샘플：

```csharp
local function notifyAccessPrivacyInfoStatusCallBack(value)
    print("=====> lua notifyAccessPrivacyInfoStatus callback: " .. value)
    upltv:initSDK(0)
 end

local function europeanUnionUserCallBack(result)
    print("=====> lua mEuropeanUnionUserCallBack result: " .. result)
    -- result: "true" means user in EU
    if result=="true" then

        -- 팝업 창으로 권한을 요청합니다.
        upltv:notifyAccessPrivacyInfoStatus(notifyAccessPrivacyInfoStatusCallBack)
    else
        -- 게임 유저가 EU 역외에 있을 경우 바로 SDK를 초기화합니다.
        upltv:initSDK(0)
    end
end

-- GDPR
local function checkGDPR()
    local e = upltv:getAccessPrivacyInfoStatus()
    print("=====> lua getAccessPrivacyInfoStatus status: " .. e)
    -- 권한 승인 요청을 하지 않았다면
    if (e==upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusUnkown) then
        -- 게임 유저가 EU 지역에 있는지 확인합니다.
        upltv:isEuropeanUnionUser(europeanUnionUserCallBack)
    else
        -- 퍼블리싱 지역이 해외일 경우 콜백은 필요가 없습니다.
        upltv:initSDK(0)
    end
end
```
### GDPR API

#### 1. 개발자가 스스로 GDPR 권한 요청 가능한 상황
권한 승인 창이 팝업되어 유저에게 데이터 수집 권한을 요청합니다. <br />
유저가 권한을 거부하면 관련 데이터 수집이 취소됩니다. UPSDK를 초기화하기 전에 호출합니다.

```csharp
function upltv:notifyAccessPrivacyInfoStatus(callback)
```
샘플：

```csharp
local mNotifyAccessPrivacyInfoStatusCallBack=function(value)
    print("=====> lua notifyAccessPrivacyInfoStatus callback: " .. value)
    if upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted==value then
          --agree

    else
          --disagree

    end
end  

upltv:notifyAccessPrivacyInfoStatus(mNotifyAccessPrivacyInfoStatusCallBack)
```


#### 2. 개발자가 스스로 게임유저에게 개인정보 사용에 관한 권한 요청을 할 수 없는 상황
외부에서 GDPR 승인을 요청할 때, 이 메소드는 UPSDK에게 승인 결과를 알려주기 위해 호출됩니다. <br />
그리고 UPSDK는 더 이상 권한 승인 요청 팝업을 하지 않습니다. UPSDK 초기화하기 전에 호출합니다.

```csharp
function upltv:updateAccessPrivacyInfoStatus(gdprPermissionEnumValue))
```


샘플：

```csharp
{

    --If the user defined, call the following method to notice  UPSDK
    upltv:updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusDefined);

    -- If the user accepted, call the following method to notice  UPSDK
    upltv:updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted);
}
```


#### 3. GDPR 승인 여부 확인하기
게임 유저의 권한 승인 결과를 얻기 위해 UPSDK를 초기화하기 전에 호출합니다.


```groovy
function upltv:getAccessPrivacyInfoStatus()
```

샘플：
```csharp
local e = upltv:getAccessPrivacyInfoStatus()
```

#### 4. 게임 유저의 지역 확인하기
UPSDK는 게임 유저에게 EU 지역 유저의 여부 확인 API를 제공해드리고 있습니다. <br />
UPSDK를 초기화하기 전에 호출합니다.


```csharp
function upltv:isEuropeanUnionUser(callback)
```
샘플：
```csharp
local function europeanUnionUserCallBack(result)
    print("=====> lua mEuropeanUnionUserCallBack result: " .. result)
    -- result: true means users in EU, apply to GDPR
    if result=="true" then
     -- TODO
    else
     -- TODO
    end
end

upltv:isEuropeanUnionUser(europeanUnionUserCallBack)
```

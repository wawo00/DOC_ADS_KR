
## GDPR
`GDPR(The General Data Protection Regulation)`은 2018년 5월 25일부터 시행된 EU(유럽연합)의 개인정보보호 법령이며, <br />
`UPSDK`는 아래와 같은 솔루션을 제공함으로써 GDPR을 준수하고 있습니다. EU 지역에서 서비스 하실 때에 안심하고 <br />
 사용하실 수 있습니다.

GDPR을 따른 UPSDK 버전은 `3.0.03`부터 적용되어 있습니다.

### GDPR 두 가지 권장 사용 방법

#### 첫 번째 방법

서비스를 최상의 상태로 구현하기 위해, 각 사의 게임 화면의 스타일에 따라 GDPR 권한 허가 창을 설정합니다. <br />
맞춤형 구현을 하셨다면, GDPR 권한 허가 결과를 UPSDK에 알려주시고 초기화를 진행합니다.

샘플：

```javascript
yourOwnGDPRCallback : function(result) {
    // result : True는 게임 유저가 승인했다는 것을 의미하고, False는 거절을 의미합니다.
    // 아래를 참조하여 승인 동기화와 UPSDK의 초기화를 완료하시기 바랍니다.
    if (result=="true") {
        upltv.updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted);
    } else {
        upltv.updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusDefined);
    }
     // updateAccessPrivacyInfoStatus ()를 호출한 후 UPSDK를 초기화합니다.
    upltv.intSdk(0);
}

europeanUnionUserCallBack : function(result) {
    cc.log("=====> js europeanUnionUserCallBack result: " + result);
    // True는 게임 유저가 EU 지역에 있다는 것을 의미하고, 그 외는 EU 역외 게임 유저를 의미합니다.
    if (result=="true") {
        // EU 지역 게임 유저라면, 권한 승인 메소드를 호출합니다.
        // callYourOwnGDPRDialog()는 가정한 메소드 입니다.
        // yourOwnGDPRCallback는 가정한 콜백입니다.
        // 이 코드를 실제 상황에 따라 치환합니다.
        callYourOwnGDPRDialog(yourOwnGDPRCallback);
    } else {
        // 게임 유저가 EU 지역에 있지 않다면 바로 UPSDK 초기화합니다.
        // 해외에 있는 것으로 가정하면 매개변수 값은 0을 전송합니다.
        upltv.intSdk(0);
    }
}
// GDPR
checkGDPR : function() {
    var e = upltv.getAccessPrivacyInfoStatus();d
    cc.log("=====> js getAccessPrivacyInfoStatus status: %d", e)
    // 권한 승인 요청을 하지 않았다면
    if (e==upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusUnkown)
    {
         // 게임 유저가 EU 지역에 있는지 판단합니다.
        upltv.isEuropeanUnionUser(europeanUnionUserCallBack);
    } else {
        // 해외에 있는 것으로 가정하면 매개변수 값은 0을 전송합니다.
        upltv.intSdk(0);
    }
}
```

#### 두 번째 방법

UPSDK에서 제공하는 표준 승인 메커니즘을 사용하신다면 아래와 같은 코드를 사용합니다.

샘플：

```javascript
notifyAccessPrivacyInfoStatusCallBack : function(value) {
    cc.log("=====> js notifyAccessPrivacyInfoStatusCallBack callback: %d ",  value);
    upltv.intSdk(0);
}

europeanUnionUserCallBack : function(result) {
    cc.log("=====> js europeanUnionUserCallBack result: " + result)
    if (result=="true"){
        // 시스템을 팝업하여 권한 승인을 요청합니다.
        upltv.notifyAccessPrivacyInfoStatus(notifyAccessPrivacyInfoStatusCallBack);
    } else {
        // 게임 유저가 EU 지역에 있지 않다면 바로 UPSDK 초기화합니다.
        // 해외에 있는 것으로 가정하면 매개변수 값은 0을 전송합니다.
       upltv.intSdk(0);
    }
}

// GDPR
checkGDPR : function() {
    var e = upltv.getAccessPrivacyInfoStatus();
    cc.log("=====> js getAccessPrivacyInfoStatus status: %d ", e);
    if (e==upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusUnkown)
    {
        upltv.isEuropeanUnionUser(europeanUnionUserCallBack);
    } else {
        upltv.intSdk(0);
    }
}
```
### GDPR API

#### 1. GDPR 권한 승인을 요청하기

권한 승인 창이 팝업되어 유저에게 데이터 수집 권한을 요청합니다. <br />
유저가 권한을 거부하면 관련 데이터 수집이 취소됩니다. UPSDK를 초기화하기 전에 호출합니다.

```javascript
 notifyAccessPrivacyInfoStatus : function(callback)
```
샘플：

```javascript
 notifyAccessPrivacyInfoStatusCallBack : function(value) {
    if (upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted==value)
    {
        //agree     

    } else {
        //disagree

    }
}
upltv.notifyAccessPrivacyInfoStatus(notifyAccessPrivacyInfoStatusCallBack);
```


#### 2. GDPR 승인을 외부에서 진행하기

외부에서 GDPR 승인을 요청할 때, 이 메소드는 UPSDK에게 승인 결과를 알려주기 위해 호출됩니다. <br />
그리고 UPSDK는 더 이상 권한 승인 요청 팝업을 하지 않습니다. UPSDK 초기화하기 전에 호출합니다.

```javascript
updateAccessPrivacyInfoStatus : function(gdprPermissionEnumValue)
```

샘플：

```javascript
{

    //disagree to collect data
    upltv.updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusDefined);

    //agree to collect data
    upltv.updateAccessPrivacyInfoStatus(upltv.GDPRPermissionEnum.UPAccessPrivacyInfoStatusAccepted);
}
```


#### 3. 개인정보 사용에 관한 권한 요청하기

게임 유저의 권한 승인 결과를 얻기 위해 UPSDK를 초기화하기 전에 호출합니다.

```javascript
 getAccessPrivacyInfoStatus : function()
```

샘플：
```javascript
var e = upltv.getAccessPrivacyInfoStatus();
```

#### 4. 게임 유저의 EU 지역 확인하기

UPSDK는 게임 유저에게 EU 지역 유저의 여부 확인 API를 제공해드리고 있습니다. <br />
UPSDK를 초기화하기 전에 호출합니다.

```javascript
isEuropeanUnionUser : function(callback)
```
샘플：
```javascript
function europeanUnionUserCallBack(result)
    if (result=="true") {
        //TODO
    } else {
        //TODO
    }
}

upltv.isEuropeanUnionUser(europeanUnionUserCallBack);
```

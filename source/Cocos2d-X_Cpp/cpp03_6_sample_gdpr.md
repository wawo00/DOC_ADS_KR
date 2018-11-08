
## GDPR
`GDPR(The General Data Protection Regulation)`은 2018년 5월 25일부터 시행된 EU(유럽연합)의 개인정보보호 법령이며, <br />
`UPSDK`는 아래와 같은 솔루션을 제공함으로써 GDPR을 준수하고 있습니다. EU 지역에서 서비스 하실 때에 안심하고 <br />
 사용하실 수 있습니다.

### GDPR 두 가지 권장 사용 방법

#### 첫 번째 방법

서비스를 최상의 상태로 구현하기 위해, 각 사의 게임 화면의 스타일에 따라 GDPR 권한 허가 창을 설정합니다. <br />
맞춤형 구현을 하셨다면, GDPR 권한 허가 결과를 UPSDK에 알려주시고 초기화를 진행합니다.

샘플：

```csharp
{
    // ...

    UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus result = UpltvBridge::getAccessPrivacyInfoStatus();
    if (result == UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus::UPAccessPrivacyInfoStatusUnkown) {
        // 게임 유저가 EU 지역에 있는지 판단합니다.
        // gdprUeropeanLocationCallback을 동기화 콜백합니다.

        UpltvBridge::isEuropeanUnionUser(gdprUeropeanLocationCallback);
    } else {
        // 해외에 있는 것으로 가정하면 매개변수 값은 0을 전송합니다.
        UpltvBridge::initSdkByZone(0);
    }

    // .....
}

void gdprUeropeanLocationCallback(bool isUeropeanUser) {
    // isUeropeanUser: True는 게임 유저가 EU 지역에 있다는 것을 의미하고, 그 외는 EU 지역외 게임 유저를 의미합니다.
    if (isUeropeanUser) {
        //  EU 지역 게임 유저라면, 권한 승인 메소드를 호출합니다.
        // callYourOwnGDPRDialog()는 가정한 메소드 입니다.
        // yourOwnGDPRCallback는 가정한 콜백입니다.
        // 이 코드를 실제 상황에 따라 치환합니다.
        callYourOwnGDPRDialog(yourOwnGDPRCallback);

    } else {
        //  게임 유저가 EU 지역에 있지 않다면 바로 UPSDK 초기화합니다.
        // 해외에 있는 것으로 가정하면 매개변수 값은 0을 전송합니다.
        UpltvBridge::initSdkByZone(0);
    }
}

private void yourOwnGDPRCallback(bool result) {
    // result : True는 게임 유저가 승인했다는 것을 의미하고, False는 거절을 의미합니다.
    // 아래를 참조하여 승인 동기화와 UPSDK의 초기화를 완료하시기 바랍니다.
    if (result) {
        UpltvBridge::updateAccessPrivacyInfoStatus(UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus::UPAccessPrivacyInfoStatusAccepted);
    } else {
        UpltvBridge::updateAccessPrivacyInfoStatus(UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus::UPAccessPrivacyInfoStatusDefined);
    }

    // updateAccessPrivacyInfoStatus ()를 호출한 후 UPSDK를 초기화합니다.
    // 해외에 있는 것으로 가정하면 매개변수 값은 0을 전송합니다.
    UpltvBridge::initSdkByZone(0);
}
```


#### 두 번째 방법

UPSDK에서 제공하는 표준 승인 프로세스를 사용하신다면 아래와 같은 코드를 사용합니다.

샘플：

```csharp

{
    // .....

    UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus result = UpltvBridge::getAccessPrivacyInfoStatus();
    if (result == UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus::UPAccessPrivacyInfoStatusUnkown) {
        // 게임 유저가 EU 지역에 있는지 판단합니다.
        // gdprUeropeanLocationCallback을 동기화 콜백합니다.
        UpltvBridge::isEuropeanUnionUser(gdprUeropeanLocationCallback);
    } else {
        // 해외에 있는 것으로 가정합니다.
        UpltvBridge::initSdkByZone(0);
    }

    // .....
}

void gdprUeropeanLocationCallback(bool result) {
    // result: True는 게임 유저가 EU 지역에 있다는 것을 의미하고, 그 외는 EU 지역외 게임 유저를 의미합니다.
    if (result) {
        // 권한 승인을 요청합니다.
        UpltvBridge::notifyAccessPrivacyInfoStatus(gdprNotifyCallback);
    } else {
        // 비 EU 게임 유저라면 바로 SDK 초기화합니다.
        // 해외에 있는 것으로 가정합니다.
        UpltvBridge::initSdkByZone(0);
    }
}

void gdprNotifyCallback(UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus result, string msg) {
    // result：게임 유저의 승인 결과
    // 결과에 상관 없이 SDK를 초기화 합니다.
    // 해외에 있는 것으로 가정합니다.
    UpltvBridge::initSdkByZone(0);
}
```

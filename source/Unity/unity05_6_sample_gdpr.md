## GDPR
`GDPR(The General Data Protection Regulation)`은 2018년 5월 25일부터 시행된 EU(유럽연합)의 개인정보보호 법령이며, <br />
`UPSDK`는 아래와 같은 솔루션을 제공함으로써 GDPR을 준수하고 있습니다. EU 지역에서 서비스 하실 때에 안심하고 <br />
 사용하실 수 있습니다.

### GDPR 샘플
#### 1. 맞춤형 구현
서비스를 최상의 상태로 구현하기 위해, 각 사의 게임 화면의 스타일에 따라 GDPR 권한 허가 창을 설정합니다. <br />
맞춤형 구현을 하셨다면, GDPR 권한 허가 결과를 UPSDK에 알려주시고 초기화를 진행합니다.

샘플:
```csharp
{
     // 이전 초기화 코드
    // AvidlyAdsSdk.init(xxxx);

    // GDPR을 위한 새로운 코드
    UPConstant.UPAccessPrivacyInfoStatusEnum result = Polymer.UPSDK.getAccessPrivacyInfoStatus();
    if (result == UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusUnkown
        || result == UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusFailed) {
        // 만약 게임 유저가 EU 지역에 있을 때
        UPSDK.isEuropeanUnionUser (new Action <bool, string>(isEuropeanUserCallback));
    } else {
        UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    }

    // .....
}

private void isEuropeanUserCallback(bool result, string msg) {
    if (result) {
           // 게임 유저가 EU에 있다면 권한 허가를 요청합니다.
            // ......
            // ......

        // callYourOwnGDPRDialog() 는 가정한 메소드 입니다.
        // yourOwnGDPRCallback()는 가정한 콜백입니다.
        // 이 코드를 실제 상황에 따라 치환합니다.
        callYourOwnGDPRDialog(yourOwnGDPRCallback);

    } else {
      //게임 유저가 EU 지역에 있지 않다면 바로 UPSDK 초기화합니다.
        UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    }
}

private void yourOwnGDPRCallback(bool result) {
    //  이 프로토콜을 승인한다면 아래의 메소드를 UPSDK에 알려줍니다.
    if (result) {
        UPSDK.updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusAccepted);
    } else {
    // 이 프로토콜을 거부한다면 아래의 메소드를 UPSDK에 알려줍니다.
        UPSDK.updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined);
    }

   // updateAccessPrivacyInfoStatus()를 호출한 이후에 초기화합니다.
    UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
}
```

#### 2. 빠른 구현
UPSDK에서 제공하는 표준 허가 프로세스를 사용하신다면 아래와 같은 코드를 사용합니다.

샘플:

```csharp

{
    // .....

    // 이전 초기화 코드
    // AvidlyAdsSdk.init(xxxx);

    // GDPR을 위한 새로운 코드
    UPConstant.UPAccessPrivacyInfoStatusEnum result = Polymer.UPSDK.getAccessPrivacyInfoStatus();
    if (result == UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusUnkown
        || result == UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusFailed) {
        UPSDK.isEuropeanUnionUser (new Action <bool, string>(isEuropeanUserCallback));
    } else {
        UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    }

    // .....
}

private void isEuropeanUserCallback(bool result, string msg) {
     // result: True는 게임 유저가 EU 지역에 있다는 것을 의미합니다.
    if (result) {
    // 권한 승인을 요청합니다.
        UPSDK.notifyAccessPrivacyInfoStatus (new Action <UPConstant.UPAccessPrivacyInfoStatusEnum, string> (accessPrivacyInforCallback));
    } else {
        UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);
    }
}

private void accessPrivacyInforCallback(UPConstant.UPAccessPrivacyInfoStatusEnum result, string msg) {
    // 승인 여부 결과이며, 결과에 상관없이 UPSDK 초기화를 진행합니다.

    UPSDK.initPolyAdSDK (UPConstant.SDKZONE_FOREIGN);

    Debug.Log ("===> accessPrivacyInforCallback Event result: " + result + "," + msg);
}
```

### GDPR API

#### 1. GDPR 권한 승인을 요청하기
권한 승인 창이 팝업되어 유저에게 데이터 수집 권한을 요청합니다. <br />
유저가 권한을 거부하면 관련 데이터 수집이 취소됩니다. UPSDK를 초기화 하기 전에 호출합니다.

```csharp
public static void notifyAccessPrivacyInfoStatus(Action <UPConstant.UPAccessPrivacyInfoStatusEnum, string> callback)
```
샘플：

```csharp
public void onBtnNotifyAccessStatus_Click() {
    Polymer.UPSDK.notifyAccessPrivacyInfoStatus (new Action <UPConstant.UPAccessPrivacyInfoStatusEnum, string>(accessPrivacyInforCallback));
}

private void accessPrivacyInforCallback(UPConstant.UPAccessPrivacyInfoStatusEnum result, string msg) {
     Debug.Log ("===> accessPrivacyInforCallback Event result: " + result + "," + msg);
}
```


#### 2.GDPR 승인을 외부에서 진행하기
이 메소드는 UPSDK에게 승인 결과를 알려주기 위해 호출됩니다. 그리고 UPSDK는 더 이상 권한 승인 요청 팝업을 <br />
하지 않습니다. UPSDK 초기화 하기 전에 호출합니다.

```csharp
public static void updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum enumValue)
```

샘플：

```csharp

public void onBtnUpdateAccessStatus_Click() {
    Polymer.UPSDK.updateAccessPrivacyInfoStatus(UPConstant.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined);
}
```


#### 3.개인정보 사용에 관한 권한 요청하기
게임 유저의 권한 승인 결과를 얻기 위해 UPSDK를 초기화하기 전에 호출합니다.

```csharp
public static UPConstant.UPAccessPrivacyInfoStatusEnum getAccessPrivacyInfoStatus()
```

샘플：
```csharp
public void onBtnGetAccessStatus_Click() {
    UPConstant.UPAccessPrivacyInfoStatusEnum e = Polymer.UPSDK.getAccessPrivacyInfoStatus();
    Debug.Log ("==> getAccessPrivacyInfoStatus :" + e);
}
```

#### 4. 게임 유저의 EU 지역 확인하기
UPSDK는 게임 유저에게 EU 지역 유저의 여부 확인 API를 제공해드리고 있습니다. <br />
UPSDK를 초기화하기 전에 호출합니다.

```csharp
public static void isEuropeanUnionUser(Action <bool, string> callback)
```
샘플：
```csharp
public void onBtnIsEuropeanUnionUser_Click() {
    Polymer.UPSDK.isEuropeanUnionUser (new Action <bool, string>(isEuropeanUserCallback));
}

private void isEuropeanUserCallback(bool result, string msg) {
    Debug.Log ("===> isEuropeanUserCallback Event result: " + result + "," + msg);
}
```

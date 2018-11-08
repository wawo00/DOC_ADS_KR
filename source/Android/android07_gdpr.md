
## GDPR

`GDPR(The General Data Protection Regulation)`은 2018년 5월 25일부터 시행된 EU(유럽연합)의 개인정보보호 법령이며, <br />
`UPSDK`는 아래와 같은 솔루션을 제공함으로써 `GDPR`을 준수하고 있습니다. EU 지역에서 서비스 하실 때에 안심하고 <br />
 사용하실 수 있습니다.

GDPR을 따른 UPSDK 버전은 `3.0.03`부터 적용되어 있습니다.


### GDPR 샘플
#### I. 맞춤형 구현
서비스를 최상의 상태로 구현하기 위해, 각 사의 게임 화면의 스타일에 따라 GDPR 권한 승인 창을 설정합니다. <br />
맞춤형 구현을 하셨다면, GDPR 권한 승인 결과를 UPSDK에 알려주시고 초기화를 진행합니다.

샘플:
```csharp
{
    // 이전 초기화 코드
    // AvidlyAdsSdk.init(xxxx);

    //  GDPR을 위한 새로운 초기화 코드
    AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum result=UPAdsSdk.getAccessPrivacyInfoStatus(MainActivity.this);
    if (result ==  AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusUnkown
        || result ==  AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined) {
         // 게임 유저가 EU 지역에 있을 경우
        UPAdsSdk.isEuropeanUnionUser (MainActivity.this,isEuropeanUserCallback);
    } else {

        UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
    }

    // .....
}

UPAdsSdk.UPEuropeanUnionUserCheckCallBack isEuropeanUserCallback=new UPAdsSdk.UPEuropeanUnionUserCheckCallBack(){
    @Override
    public void isEuropeanUnionUser(boolean result) {
        if (result) {
            // EU 지역 게임 유저에게 GDPR권한 요청합니다.
            // ......
            // ......

            // 이 메소드를 사용합니다.
            // 게임 유저가 권한을 거부하였다면 아래의 매소드를 통하여 UPSDK에 알려주시기 바랍니다.
            UPAdsSdk.updateAccessPrivacyInfoStatus(this, AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined);

            //  게임 유저가 권한을 승인하였다면 아래의 매소드를 통하여 UPSDK에 알려주시기 바랍니다.
            UPAdsSdk.updateAccessPrivacyInfoStatus(this, AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusAccepted);

            // updateAccessPrivacyInfoStatus() 호출 이후 초기화
            UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);

        } else {
            // 게임 유저가 EU 역외에 있을 경우, UPSDK를 바로 초기화 합니다
            UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
        }
    }
};

```

#### II. 빠른 구현
UPSDK에서 제공하는 표준 승인 프로세스를 사용하신다면 아래와 같은 코드를 사용합니다.

샘플：

```csharp

{
    // 이전 초기화 코드
    // AvidlyAdsSdk.init(xxxx);

    // GDPR을 위한 새로운 초기화 코드
    AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum result=UPAdsSdk.getAccessPrivacyInfoStatus(MainActivity.this);
    if (result ==  AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusUnkown
        || result ==  AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined) {

        //게임 유저가 EU 지역에 있을 경우
        UPAdsSdk.isEuropeanUnionUser (MainActivity.this,isEuropeanUserCallback);
    } else {
        UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
    }
    // .....
}

UPAdsSdk.UPEuropeanUnionUserCheckCallBack isEuropeanUserCallback = new UPAdsSdk.UPEuropeanUnionUserCheckCallBack(){
    @Override
    public void isEuropeanUnionUser(boolean result) {
        if (result) {
            // result: true는 게임 유저가 EU 지역에 있다는 것을 의미합니다.
            // ......
            // ......

            // EU 지역 게임 유저에게 GDPR권한 요청합니다.
             UPAdsSdk.notifyAccessPrivacyInfoStatus(MainActivity.this,myAccessPrivacyStatusInfoCallBack);
        } else {
            // 게임 유저가 EU 역외에 있을 경우, UPSDK를 바로 초기화 합니다.
            UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
        }
    }
};

private UPAdsSdk.UPAccessPrivacyInfoStatusCallBack myAccessPrivacyStatusInfoCallBack = new UPAdsSdk.UPAccessPrivacyInfoStatusCallBack() {
    @Override
    public void onAccessPrivacyInfoAccepted() {
        Log.i(TAG, "onAccessPrivacyInfoAccepted:");
        // ......
        // ......
        UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
    }

    @Override
    public void onAccessPrivacyInfoDefined() {
        Log.i(TAG, "onAccessPrivacyInfoDefined:");
        // ......
        // ......
        UPAdsSdk.init(MainActivity.this, UPAdsSdk.UPAdsGlobalZone.UPAdsGlobalZoneForeign);
    }

};
```
### GDPR API

#### 1. 개발자가 스스로 GDPR 권한 요청 가능한 상황

권한 부여 창이 팝업되어 유저에게 데이터 수집 권한을 요청합니다. <br />
유저가 권한을 거부하면 관련 데이터 수집이 취소됩니다. UPSDK를 초기화 하기 전에 호출하시기 바랍니다.

```csharp
public static void notifyAccessPrivacyInfoStatus(final Context context, final UPAdsSdk.UPAccessPrivacyInfoStatusCallBack callback)
```
샘플：

```csharp
UPAdsSdk.notifyAccessPrivacyInfoStatus(MainActivity.this,myAccessPrivacyStatusInfoCallBack);

private UPAdsSdk.UPAccessPrivacyInfoStatusCallBack myAccessPrivacyStatusInfoCallBack =new UPAdsSdk.UPAccessPrivacyInfoStatusCallBack() {
   @Override
   public void onAccessPrivacyInfoAccepted() {
       Log.i(TAG, "onAccessPrivacyInfoAccepted: ");
   }

   @Override
   public void onAccessPrivacyInfoDefined() {
       Log.i(TAG, "onAccessPrivacyInfoDefined");
   }
};

```


#### 2. 개발자가 스스로 게임유저에게 개인정보 사용에 관한 권한 요청을 할 수 없는 상황
외부에서 GDPR 권한을 요청할 경우 이 메소드를 통해 UPSDK에 유저 권한 결과를 업로드합니다. <br />
UPSDK는 더 이상 권한 부여 팝업 창을 띄우지 않습니다. UPSDK를 초기화하기 전에 호출하시기 바랍니다.

```csharp
public static void updateAccessPrivacyInfoStatus(final Context context,UPConstant.UPAccessPrivacyInfoStatusEnum enumValue)
```

Sample：

```csharp
/**
* 데이터 수집을 승인
*/
UPAdsSdk.updateAccessPrivacyInfoStatus(this, AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusAccepted);

/**
 * 데이터 수집을 거부
 */
UPAdsSdk.updateAccessPrivacyInfoStatus(this, AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum.UPAccessPrivacyInfoStatusDefined);

```


#### 3. GDPR 승인 여부 확인하기
게임유저의 권한 승인 여부를 UPSDK를 초기화하기 전에 호출하실 수 있습니다.

```groovy
public static UPAccessPrivacyInfoStatusEnum getAccessPrivacyInfoStatuss(Context context)
```

샘플：
```csharp
AccessPrivacyInfoManager.UPAccessPrivacyInfoStatusEnum result=UPAdsSdk.getAccessPrivacyInfoStatus(MainActivity.this);
```

#### 4. 게임유저의 지역 확인하기
게임유저가 EU 지역에 있는지에 대한 여부를 UPSDK를 초기화하기 전에 호출하실 수 있습니다.

```csharp
public static void isEuropeanUnionUser(Context context, UPAdsSdk.UPEuropeanUnionUserCheckCallBack callback)
```
샘플：
```csharp
 UPAdsSdk.isEuropeanUnionUser(MainActivity.this, new UPAdsSdk.UPEuropeanUnionUserCheckCallBack() {
    @Override
    public void isEuropeanUnionUser(boolean isEurope) {
        if (isEurope) {
            //TODO
        } else {
            //TODO
        }
    }
});
```

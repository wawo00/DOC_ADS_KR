## GDPR

`GDPR(The General Data Protection Regulation)`은 2018년 5월 25일부터 시행된 EU(유럽연합)의 개인정보보호 법령이며, <br />
`UPSDK`는 아래와 같은 솔루션을 제공함으로써 `GDPR`을 준수하고 있습니다. EU 지역에서 서비스 하실 때에 안심하고 <br />
 사용하실 수 있습니다.

GDPR을 따른 UPSDK 버전은 `3.0.03`부터 적용되어 있습니다.

### GDPR 샘플
#### I 맞춤형 구현
서비스를 최상의 상태로 구현하기 위해, 각 사의 게임 화면의 스타일에 따라 GDPR 권한 승인 창을 설정합니다. <br />
맞춤형 구현을 하셨다면, GDPR 권한 승인 결과를 UPSDK에 알려주시고 초기화를 진행합니다.

샘플：
```objective-c
{
    // 이전 초기화 코드
    // AvidlyAdsSdk.init(xxxx);

    // GDPR을 위한 새로운 코드
    UPAccessPrivacyInfoStatus result1 = [UPSDK getCurrentAccessPrivacyInfoStatus];
    if (result1 == UPAccessPrivacyInfoStatusNone) {
        [UPSDK checkIsEuropeanUnionUser:^(BOOL isEuropeanUnion) {
            if (isEuropeanUnion) {
             // 만약 유저가 EU 지역에 있을 때
                [Test yourOwnMethod:nil completion:^(BOOL isAccepted) {
                    NSLog(@"TODO");
                    if (isAccepted) {
                        [UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusAccepted];
                    }
                    else {
                        [UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusDenied];
                    }

                    [UPSDK initSDK:UPSDKGlobalZoneForeign];
                }];
            }
            else {

                [UPSDK initSDK:UPSDKGlobalZoneForeign];
            }
        }];
    }
    else {

        [UPSDK initSDK:UPSDKGlobalZoneForeign];
    }
}
```
#### II. 빠른 구현
UPSDK에서 제공하는 표준 승인 프로세스를 사용하신다면 아래와 같은 코드를 사용합니다.


샘플：
```objective-c
{
    // 이전 초기화 코드
    // AvidlyAdsSdk.init(xxxx);

    // GDPR을 위한 새로운 코드
    UPAccessPrivacyInfoStatus result = [UPSDK getCurrentAccessPrivacyInfoStatus];
    if (result == UPAccessPrivacyInfoStatusNone) {

        [UPSDK checkIsEuropeanUnionUser:^(BOOL isEuropeanUnion) {
            if (isEuropeanUnion) {
                // 만약 게임 유저가 EU 지역에 있을 때
                [UPSDK requestAuthorizationWithAlert:nil completion:^(BOOL isAccepted) {
                    [UPSDK initSDK:UPSDKGlobalZoneForeign];
                }];
            }
            else {
                [UPSDK initSDK:UPSDKGlobalZoneForeign];
            }
        }];
    }
    else {
        [UPSDK initSDK:UPSDKGlobalZoneForeign];
    }
}
```
### GDPR API

#### 1. 개발자가 스스로 GDPR 권한 요청 가능한 상황

권한 부여 창이 팝업되어 유저에게 데이터 수집 권한을 요청합니다. <br />
유저가 권한을 거부하면 관련 데이터 수집이 취소됩니다. UPSDK를 초기화하기 전에 호출하시기 바랍니다.

```objective-c

+ (void)updateAccessPrivacyInfoStatus:(UPAccessPrivacyInfoStatus)status;
```

`UPAccessPrivacyInfoStatus`:
```objective-c
typedef NS_ENUM (NSInteger, UPAccessPrivacyInfoStatus) {
    UPAccessPrivacyInfoStatusNone = 0,      //승인 여부 알지 못함
    UPAccessPrivacyInfoStatusAccepted = 1,  //승인
    UPAccessPrivacyInfoStatusDenied = 2,    //거부
};
```

**주의: `UPAccessPrivacyInfoStatusNone `를 전송하지 않습니다.**

샘플:

```objective-c

[UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusAccepted];
//SDK init
[UPSDK initSDK:UPSDKGlobalZoneForeign];
```

---------

#### 2. 게임유저가 EU 지역 유저 여부를 판단할 수 없는 상황
UPSDK는 게임 유저에게 EU 지역 유저의 여부 확인 API를 지원해드리고 있습니다. <br />
UPSDK를 초기화하기 전에 호출하시기 바랍니다.

```objective-c

+ (void)checkIsEuropeanUnionUser:(void (^)(BOOL isEuropeanUnion))completionBlock;
```



샘플:

```objective-c
[UPSDK checkIsEuropeanUnionUser:^(BOOL isEuropeanUnion) {
    if (isEuropeanUnion) {
    }
    else {
}];
```

---------

#### 3. 개발자가 스스로 게임유저에게 개인정보 사용에 관한 권한 요청을 할 수 없는 상황

UPSDK는 다음과 같이 Alter를 사용하여 유저의 개인정보를 사용 할 수 있는 권한 요청 API를 제공해드리고 있습니다.

```objective-c
/**
 게임 유저에게 개인 정보 접근 권한 요청을 합니다.

 @param viewController
 @param completionBlock Callback,  YES는 게임 유저가 동의했다는 것이고, NO는 거부했다는 것을 의미합니다.
 */
+ (void)requestAuthorizationWithAlert:(UIViewController *)viewController completion:(void (^)(BOOL isAccepted))completionBlock;
```


샘플:

```objective-c
[UPSDK requestAuthorizationWithAlert:vc completion:^(BOOL isAccepted) {
    if (isAccepted) {

    }
    else {

    }
}];
```

--------

#### 4. GDPR 승인 여부 확인하기

UPSDK는 현재 GDPR 승인 여부를 확인할 수 있도록 현재 GDPR 권한 승인 여부 확인 API를 제공해드리고 있습니다.

```objective-c
/**

 @GDPR 승인 상황을 리턴합니다.
 */
+ (UPAccessPrivacyInfoStatus)getCurrentAccessPrivacyInfoStatus;
```


리턴 값 :
`UPAccessPrivacyInfoStatusAccepted`는 유저가 동의 했다는 것을 나타내고, <br />
`UPAccessPrivacyInfoStatusDenied`는 유저가 거부 했다는 것을 나타냅니다. <br />
`UPAccessPrivacyInfoStatusNone`는 상황을 알 수 없거나 권한요청이 발송되지 않았다는 것을 나타냅니다.

#### 5. 전체 샘플

```objective-c
//Step1
[UPSDK checkIsEuropeanUnionUser:^(BOOL isEuropeanUnion) {
    if (isEuropeanUnion) {

        // Step2 개인정보 사용에 대한 승인을 요청합니다.
        [UPSDK requestAuthorizationWithAlert:vc completion:^(BOOL isAccepted) {
            if (isAccepted) {

                // Step3 UPSDK에게 GDPR 승인 여부를 보내줍니다(동의)
                [UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusAccepted];
            }
            else {

                 // Step3 UPSDK에게 GDPR 승인 여부를 보내줍니다(거절)
                [UPSDK updateAccessPrivacyInfoStatus:UPAccessPrivacyInfoStatusDenied];
            }

            // Step4 초기화
            [UPSDK initSDK:UPSDKGlobalZoneForeign];
        }];
    }
    else {

        // Step4 초기화
        [UPSDK initSDK:UPSDKGlobalZoneForeign];
    }
}];
```

Step1과 Step2는 사용자 경험(UX)을 개선하기 위해 개발자께서 각 사에 맞게 구현하실 것을 추천드립니다. <br />
또는, UPSDK에서 제공하는 이에 맞는 API를 사용하여 해결할 수도 있습니다.

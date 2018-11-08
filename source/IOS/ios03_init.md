## SDK 초기화

Xcode에 대한 부분입니다. 이 부분에서는 Xcode를 사용한 예시만 설명드리고 있습니다. <br />
다른 개발 툴을 사용하고 계신 분들께 불편을 끼쳐 드려 죄송합니다.

AppDelegate를 사용하고 계시다면 아래와 같은 순서대로 SDK 초기화를 합니다.

1. 아래의 참조를 `AppDelegate.m`에 추가합니다.

    ```objective-c
    #import    <UPSDK/UPSDK.h>
    ```

2. SDK 초기화 설정하기

```objective-c
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    [UPSDK initSDK];

    // your other code

    return YES;
}
```

희망 퍼블리시 위치를 아래와 같이 설정하실 수 있습니다.
```objective-c
/**
SDK 초기화 (퍼블리시 지역)
@param zone, 퍼블리시 지역
 */
+ (void)initSDK:(UPSDKGlobalZone)zone;
```

`UPSDKGlobalZone`for example

```objective-c
typedef NS_ENUM(NSUInteger, UPSDKGlobalZone) {
    UPSDKGlobalZoneForeign = 0,     //해외
    UPSDKGlobalZoneDomestic = 1,    //중국
    UPSDKGlobalZoneAuto = 2,        //ip에 의해 자동으로 설정
};
```
앱을 중국에서 퍼블리시 하시려면 아래와 같이 입력합니다.

```objective-c
[UPSDK initSDK:UPSDKGlobalZoneDomestic];
```

중국을 제외한 전 세계로 퍼블리시 하시려면 아래와 같이 입력합니다.

```objective-c
[UPSDK initSDK: UPSDKGlobalZoneForeign];
```

지역을 특정하지 않으시려면 아래와 같이 입력합니다.

```objective-c
[UPSDK initSDK: UPSDKGlobalZoneAuto];
```

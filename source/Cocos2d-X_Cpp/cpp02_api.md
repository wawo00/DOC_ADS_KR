## UPSDK cocos2d-X Cpp API

전체 **UpltvCppBridge** 디렉토리에는 * upltvbridge.h * 파일, 주로 외부 액세스 API 및 콜백 기능을 정의합니다.

```cpp
#include "UpltvBridge.h"
```

#### 1. 콜백 기능 정의

```cpp
typedef void (*UpltvSdkBoolCallback)(bool r);
typedef void (*UpltvSdkStringBoolCallback)(const char* cpadid, bool r);
typedef void (*UpltvSdkStringCallback_1)(UpltvAdEventEnum::AdEventType type, const char* cpadid);
typedef void (*UpltvSdkStringCallback_2)(const char* cpadid, const char* msg);
typedef void (*UpltvSdkGDPRNotifyCallback)(UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus status, const char* msg);
```
#### 2. 열거형

###### 2.1  광고 콜백 이벤트

```cpp
class UpltvAdEventEnum
{
public:
    enum class AdEventType
    {
        // 리워드 동영상 콜백 이벤트 유형
        VIDEO_EVENT_DID_SHOW = 0,  //리워드 동영상 실행하기
        VIDEO_EVENT_DID_CLICK = 1,  //리워드 동영상 클릭하기
        VIDEO_EVENT_DID_CLOSE = 2,  //리워드 동영상 닫기
        VIDEO_EVENT_DID_GIVEN_REWARD = 3,  //리워드 동영상을 통해 보상하기
        VIDEO_EVENT_DID_ABANDON_REWARD = 4,  //보상 조건을 충족하지 못했으므로 보상하지 않기

        // 삽입 광고 콜백 이벤트 유형
        INTERSTITIAL_EVENT_DID_SHOW = 5,   //삽입 광고 실행하기
        INTERSTITIAL_EVENT_DID_CLICK = 6,  //삽입 광고 클릭하기        
        INTERSTITIAL_EVENT_DID_CLOSE = 7,  // 삽입 광고 닫기

        // 배너 광고 콜백 이벤트 유형
        BANNER_EVENT_DID_SHOW = 8,  //배너 광고 실행하기
        BANNER_EVENT_DID_CLICK = 9,  //배너 광고 클릭하기
        BANNER_EVENT_DID_REMOVED = 10,  //배너 광고 삭제하기

        // exit 이벤트 유형
        EXITAD_EVENT_DID_SHOW = 11,
        EXITAD_EVENT_DID_CLICK = 12,
        EXITAD_EVENT_DID_CLICKMORE = 13,
        EXITAD_EVENT_DID_EXIT = 14,
        EXITAD_EVENT_DID_CANCEL = 15,
    };

};
```

###### 2.2 GDPR 승인 상황

```cpp
class UpltvGDPRPermissionEnum {

public:
    enum class UPAccessPrivacyInfoStatus
    {
        UPAccessPrivacyInfoStatusUnkown = 0,
        UPAccessPrivacyInfoStatusAccepted = 1,
        UPAccessPrivacyInfoStatusDefined = 2,
    };

};
```

### 3. API 도입

#### 3.1 SDK 초기화

```cpp
/**
* UPSDK를 초기화합니다
* SDK의 다른 API 인터페이스를 사용하기 전에 SDK 초기화를 완료하시기 바랍니다
* @param zone 제품 배포 지역, 0: 중국을 제외한 해외, 1: 중국,  2: IP에 의해 자동 설정
static void initSdkByZone(int zone);

/**
* UPSDK를 초기화합니다
* SDK의 다른 API 인터페이스를 사용하기 전에 SDK 초기화를 완료하시기 바랍니다
* @param zone 제품 배포 지역, 0: 중국을 제외한 해외, 1: 중국,  2: IP에 의해 자동 설정
* @param callback SDK 초기화가 완료된 후 콜백 인터페이스, 콜백 인터페이스는 Boolean 매개변수 콜백(Boolean)을 포함하며, 성공 및 실패를 나타냅니다
*/
static void initSdkByZoneWithCall(int zone, UpltvSdkBoolCallback callback);

/**
* 게임 혹은 앱이 실행되고 있을 때, 아래를 호출하여 UPSDK에게 상태 변동을 알려줍니다
* 백그라운드에 진입할 때 hasfocus 매개변수를 false로 설정하고, 포그라운드로 진입 할 때에는  true로 설정합니다
* cocos2d-x에서, 현재 프로젝트 AppDelegate.cpp에 이 메소드를 추가합니다. 자세한 내용은 샘플 -> UPSDK 초기화 -> onApplicationFocus() 추가하기 를 참고합니다
* @param hasfocus
*/
static void onApplicationFocus(bool hasfocus);

```
#### 3.2 A/B 테스트 초기화 및 데이터 가져오기

```cpp
/**
* A/B 테스트를 할 때, SDK 초기화 후 A/B테스트 초기화하는데 아래 메소드를 호출합니다.
* @param gameAccountId          const char*유형, 게임 유저의 id (필수사항)
* @param completeTask           bool 유형, 게임에서 튜토리얼 완수 여부
* @param isPaid                 int 유형, 결제 유저 판단, 0이면 비결제 유저
* @param promotionChannelName   const char* 유형, 광고 채널, 없으면 "" 전송 가능
* @param gender                 const char* 유형，성별 "M", "F"，모르면 "" 전송가능
* @param age                    int 유형, 연령, -1은 unknown
* @param tags                   vector 유형, 태그, 없으면 null 전송 가능
*/
static void initAbtConfigJson(const char* gameAccountId, bool completeTask, int isPaid,
                                  const char* promotionChannelName, const char* gender, int age, vector<string> *tags);

/**
* A/B 테스트 초기화가 완료된 후, 결과는 이 메소드를 통해 가져옵니다.
* 가져온 결과가 올바른지 확인하기 위해 initAbtConfigJson() 콜타임을 2초 이상 딜레이 하기를 권장합니다.
* @param cpPlaceId  string 유형, 광고 placement ID
* @return const char*
*/
static const char* getAbtConfig(const char* cpPlaceId);
```
#### 3.3 리워드 동영상 광고(Rewarded Video Ad)

```cpp
/**
* 리워드 동영상의 디버그 인터페이스를 엽니다.
*/
static void showRewardVideoDebugUI();

/**
* 리워드 동영상 로드 콜백 인터페이스 메소드를 설정합니다.
* 현재 삽입 광고를 수신하기 위해 성공 또는 실패 결과를 로드합니다.
* 이 인터페이스는 콜백이 발생하면 자동으로 릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.
* @param successCall 동영상이 성공적으로 로드됐을 때 콜백을 활성화 합니다.(cpadid, msg)
* @param failCall    동영상이 성공적으로 로드되지 못했을 때 콜백을 활성화 합니다.(cpadid, msg)
*/
static void setRewardVideoLoadCallback(UpltvSdkStringCallback_2 successCall, UpltvSdkStringCallback_2 failCall);

/**
* 리워드 동영상 광고의 이벤트(클릭, 닫기, 보상 등) 콜백을 수신하도록 콜백 인터페이스를 설정합니다.
* 실행화면 인터페이스에 대한 참조는 내부적으로 저장되며 릴리즈되지 않습니다.
* 콜백 인터페이스 호출 방법, 콜백(유형, cpid)와 디스플레이 콜백, 클릭 콜백, 닫기 콜백, 성공적인 콜백, 실패한 콜백이 있습니다.
* @param callback(type, cpid)
*/
static void setRewardVideoShowCallback(UpltvSdkStringCallback_1 callback);

/**
* 리워드 동영상 광고가 준비되었는지 판단합니다.
* Boolean 결과를 동기화 리턴합니다. True는 광고가 디스플레이 준비가 되었다는 것을 의미하고, false 는 광고가 실행되지 않거나, 요청 중에 있다는 것을 의미합니다.
* 이 메소드는 showRewardVideo(cpPlaceId) 전에 호출합니다.
* @return bool
*/
static bool isRewardReady();

/**
* 리워드 동영상 광고 실행하기
* @param cpPlaceId 동영상 디스플레이의 광고 공간을 활성화합니다, 이는 사업 시작점이자 수익원을 쉽게 구별하기 위해 사용됩니다.
*/
static void showRewardVideo(const char* cpPlaceId);
```

#### 3.4 삽입 광고(Interstitial Ad)

```cpp
/**
* 삽입 광고 디버그 인터페이스를 엽니다.
*/
static void showInterstitialDebugUI();

/**
* 재생 시간 동안 클릭 및 닫기 등의 이벤트 콜백을 수신하는 데 사용되는 삽입 광고의 디스플레이를 위한 콜백 인터페이스를 설정합니다.
* 삽입 광고는 콜백 인터페이스에 대한 참조가 내부적으로 저장되고 릴리스되지 않았음을 나타냅니다.
* 콜백 인터페이스 호출 메소드, 콜백(유형, cpid), 유형은 이벤트 유형을 나타내며, 재생 콜백, 클릭 콜백, 닫기 콜백이 있습니다


* @param cpPlaceId
* @param callback
*/
static void setInterstitialShowCallback(const char* cpPlaceId, UpltvSdkStringCallback_1 callback);

/**
* 삽입 광고 로드 콜백 인터페이스 메소드 설정하기.
* 삽입 광고를 수신하기 위해 성공 또는 실패 결과를 로드합니다.
* 이 인터페이스는 콜백이 발생하면 자동으로 릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.
* @param successCall 동영상이 성공적으로 로드됐을 때 콜백을 활성화 합니다.(cpadid, msg)
* @param failCall    동영상이 성공적으로 로드되지 못했을 때 콜백을 활성화 합니다.(cpadid, msg)
*/
static void setInterstitialLoadCallback(const char* cpPlaceId, UpltvSdkStringCallback_2 loadSuccess, UpltvSdkStringCallback_2 loadFail);

/**
* placementID에 따른 삽입 광고가 준비되었는지 판단합니다.
* r은 광고 로드의 결과이며, True는 광고가 디스플레이 준비가 되었다는 것을 의미하고, false 는 광고가 실행되지 않거나, 요청 중에 있다는 것을 의미합니다.
* @param cpPlaceId 광고 placementID이니, 상세하고 정확한 값을 가져야 하며, 공백으로 남겨두지 않습니다.
* @param callback
*/
static void isInterstitialReadyAsyn(const char* cpPlaceId, UpltvSdkBoolStringCallback callback);

/**
* 삽입 광고가 준비되었는지 판단합니다.
* Boolean 결과를 동기화 리턴합니다. True는 광고가 디스플레이 준비가 되었다는 것을 의미하고, false 는 광고가 실행되지 않거나, 요청 중에 있다는 것을 의미합니다.
* 이 메소드는 초기화 광고 실행 전에 호출합니다.(cpPlaceId)
* @param cpPlaceId
* @return bool
*/
static bool isInterstitialReady(const char* cpPlaceId);

/**
 * 광고 placement ID에 따른 삽입 광고를 실행합니다.
 * @param cpPlaceId
 */
static void showInterstitialAd(const char* cpPlaceId);
```

#### 3.5 배너 광고(Banner Ad)

```cpp
/**
* 배너 광고 placement ID의 콜백 인터페이스를 설정합니다. 콜백 인터페이스는 저장되어
upltv:removeBannerAdAt(cpPlaceId) 를 호출할 때 삭제됩니다.
* @param cpPlaceId
* @param callback
*/
static void setBannerShowCallback(const char* cpPlaceId, UpltvSdkStringCallback_1 callback);

/**
* 스크린 상단에 배너 광고를 실행합니다.
* @param cpPlaceId
*/
static void showBannerAdAtTop(const char*cpPlaceId);

/**
* 스크린 하단에 배너 광고를 실행합니다.
* @param cpPlaceId
*/
static void showBannerAdAtBottom(const char*cpPlaceId);

/**
* 현재 스크린 상단 배너를 숨깁니다.
*/
static void hideBannerAdAtTop();

/**
* 현재 스크린 하단 배너를 숨깁니다.
*/
static void hideBannerAdAtBottom();

/**
* 광고 placementID에 따른 배너 광고를 삭제합니다.
* @param cpPlaceId
*/
static void removeBannerAdAt(const char*cpPlaceId);

/**
* 상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.
* @param padding: 상단 배너의 상쇄 값, 예를 들어 32를 입력하면 32 픽셀만큼 아래로 이동합니다.
* 이 기능은 Android 플랫폼에서는 지원되지 않습니다.
* 3002 버전부터 지원하고 있습니다.
*/
static void setTopBannerPadingForIphonex(int padding);
```

#### 3.6 참고 설명
```cpp
/**
* SDK 초기화할 때, 자동 광고 로딩이 아닌 게임에 따라 맞춤형 설정을 통해 로딩을 하고자 하는 경우
* 조건:  SDK가 임의로 자동 광고를 로딩을 할수 없거나 UPLTV의 백그라운드에서 이러한 기능을 꺼 두었을 경우.
* 위의 조건에 부합하지 않을 경우 아래의 내용이 호출되어도 SDK는 자동으로 이 항목을 무시합니다.
*/
static void loadAdsByManual();

/**
* 앱 나가기
*/
static void exitApp();
```

#### 4. GDPR
`GDPR "The General Data Protection Regulation"`은 2018년 5월 25일부터 시행된 EU(유럽연합)의 개인정보보호 법령이며, <br />
EU 지역  고객분께서 본 제품을 사용하시더라도 `UPSDK`는 아래와 같은 솔루션을 제공함으로써 `GDPR`을 준수하고 있습니다.

```cpp
/**
  * 이 메소드는 게임 유저 승인 결과를 UPSDK와 동기화하기 위해 외부 GDPR 인증을 수행할 때 호출됩니다.
  * UPSDK 초기화하기 전에 호출합니다.
  * @param status
  * 3003 및 상위 버전에서 지원합니다.
  */
 static void updateAccessPrivacyInfoStatus(UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus status);

/**
 * 현재 GDPR 승인 상태를 가져오고 UpltvGDPRPermissionEnum 값을 리턴합니다.
 * UPSDK 초기화 전에 호출합니다.
 * 3003 및 상위 버전에서 지원합니다.
*/
 static UpltvGDPRPermissionEnum::UPAccessPrivacyInfoStatus getAccessPrivacyInfoStatus();

/**
  * 권한 부여 창이 팝업되어 유저에게 데이터 수집 권한을 요청합니다.
  * 유저가 권한을 거부하면 관련 데이터 수집이 취소됩니다.
  * UPSDK를 초기화하기 전에 호출하시기 바랍니다.
  * @param callback
  * 3003 및 상위 버전에서 지원합니다.
  */
 static void notifyAccessPrivacyInfoStatus(UpltvSdkGDPRNotifyCallback callback);

/**
  * 게임 유저가 EU 지역에 있는지 판단합니다
  * UPSDK 초기화 전에 호출합니다.
  * 3003 및 상위 버전에서 지원합니다.
  */
static void isEuropeanUnionUser(UpltvSdkBoolCallback callback);
```

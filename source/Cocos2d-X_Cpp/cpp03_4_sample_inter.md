## 삽입 광고(Interstitial Ad) API

### 1. 삽입 광고 로드 콜백 설정하기

현재 삽입 광고를 수신하기 위해 성공 또는 실패 결과를 로드합니다. 이 인터페이스는 콜백이 발생하면 자동으로 <br />
릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.

```cpp
/**
* @param cpPlaceId   placement ID
* @param loadSuccess 동영상이 성공적으로 로드됐을 때 콜백을 활성화 합니다.(cpadid, msg)
* @param loadFail    동영상이 성공적으로 로드되지 못했을 때 콜백을 활성화 합니다.(cpid, message)
*/
static void setInterstitialLoadCallback(const char* cpPlaceId, UpltvSdkStringCallback_2 loadSuccess, UpltvSdkStringCallback_2 loadFail);
```

예시：

```cpp
void iLLoadCallSuccess(const char *cpid, const char *msg) {
    log("====> iLLoadCallSuccess at cpid: %s", (cpid != nullptr ? cpid : ""));
}

void iLLoadCallFail(const char *cpid, const char *msg) {
    log("====> iLLoadCallFail  at cpid: %s", (cpid != nullptr ? cpid : ""));
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//삽입 광고의 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
                 UpltvBridge::setInterstitialLoadCallback(ilkey.c_str(), iLLoadCallSuccess, iLLoadCallFail);
            }
                break;
        }
   }
}
```
### 2. 삽입 광고 실행 콜백 설정하기

콜백의 실행을 수신할 수 있는 삽입 광고의 디스플레이를 위해 콜백 인터페이스를 설정합니다. <br />
삽입 광고는 콜백 인터페이스에 대한 참조가 저장되고 릴리즈되지 않음을 나타냅니다.

```cpp
/**
* @param cpPlaceId placementid
* @param callback  callback
*/
static void setInterstitialShowCallback(const char* cpPlaceId, UpltvSdkStringCallback_1 callback);
```

예시：

```cpp
void iLShowCallback(UpltvAdEventEnum::AdEventType type, const char *cpid) {
    string s = "unkown";
    switch (type) {
        case UpltvAdEventEnum::AdEventType::INTERSTITIAL_EVENT_DID_SHOW:{
            s = "Did_Show";
        }
            break;
        case UpltvAdEventEnum::AdEventType::INTERSTITIAL_EVENT_DID_CLICK:{
            s = "Did_Click";
        }
            break;
        case UpltvAdEventEnum::AdEventType::INTERSTITIAL_EVENT_DID_CLOSE:{
            s = "Did_Close";
        }
            break;
    }
    log("====> interstitial ad showCall event: %s, at cpid: %s", (s.c_str() != nullptr ? s.c_str() : ""), cpid?cpid:"");
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//삽입 광고의 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
                 UpltvBridge::setInterstitialShowCallback(ilkey.c_str(), iLShowCallback);
            }
                break;
        }
   }
}
```
### 3. 삽입 광고 준비 여부 확인하기

삽입 광고의 준비 여부는 매개변수 placementid에 의해 판단되고, Bool 결과를 동시에 리턴합니다. True는 광고가 <br />
디스플레이 준비가 되었다는 것을 의미하고, False는 광고가 실행되지 않고, 여전히 요청 상태에 있다는 것을 의미합니다.

```cpp
 /**
* @param cpPlaceId
* @return bool
*/
static bool isInterstitialReady(const char* cpPlaceId);
```

예시：

```cpp
void iLReadyCallback(const char* cpPlaceId, bool r) {
    log("====> cpp iLReadyCallback(%s, %s)", cpPlaceId, (r ? "true" : "false"));
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//삽입 광고의 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
                UpltvBridge::isInterstitialReadyAsyn(ilkey.c_str(), iLReadyCallback);
            }
                break;
        }
   }
}
```
### 4. 삽입 광고 실행하기

매개변수 `Placement ID`에 따른 삽입 광고가 실행됩니다.

```cpp
/**
* @param cpPlaceId
*/
static void showInterstitialAd(const char* cpPlaceId);
```

예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//삽입 광고의 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
                if (UpltvBridge::isInterstitialReady(ilkey.c_str())) {
                    UpltvBridge::showInterstitialAd(ilkey.c_str());
                }
            }
                break;
        }
   }
}
```
### 5. 삽입 광고의 디버그 페이지

개발자가 광고의 사용 및 로딩 상태를 볼 수 있도록 하기 위해, UPLTV는 스크린 광고에 대한 디버그 페이지를 제공합니다. <br />
광고의 매개변수 및 로딩 상태 구조를 확인하실 수 있습니다.

```cpp
static void showInterstitialDebugUI();
```

예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        string ilkey = "Interstitial_LevelPass";//삽입 광고의 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
                UpltvBridge::showInterstitialDebugUI();
            }
                break;
        }
   }
}
```

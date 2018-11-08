## 배너 광고(Banner Ad)

배너 광고(Banner Ad)는 상단 배너와 하단 배너로 구분되며, CppPlugin은 디스플레이, 숨기기, 제거와 같은 이벤트 콜백 <br />
및 인터페이스를 제공하여 배너 광고의 구현을 더욱 단순화 합니다.

### 1. 배너 콜백 콜백

배너 광고는 실행 및 클릭, 이벤트 콜백 인터페이스의 디스플레이를 설정해야 합니다. 콜백 인터페이스는 Plug-in에 의해 <br />
내부적으로 저장되므로 여러 번 설정할 필요가 없습니다. upltv:removeBannerAdAt(cpPlaceId)라는 호출만 삭제됩니다.

```cpp
/**
* @param cpPlaceId banner placementID
* @param callback
*/
static void setBannerShowCallback(const char* cpPlaceId, UpltvSdkStringCallback_1 callback);
```
예시：

```cpp
void bannerCallback(UpltvAdEventEnum::AdEventType type, const char *cpid) {
    string s = "unkown";
    switch (type) {
        case UpltvAdEventEnum::AdEventType::BANNER_EVENT_DID_SHOW:{
            s = "Did_Show";
        }
            break;
        case UpltvAdEventEnum::AdEventType::BANNER_EVENT_DID_CLICK:{
            s = "Did_Click";
        }
            break;
        case UpltvAdEventEnum::AdEventType::BANNER_EVENT_DID_REMOVED:{
            s = "Did_Removed";
        }
            break;
    }
    log("====> Banner ad call event: %s, at cpid: %s", (s.c_str() != nullptr ? s.c_str() : ""), cpid?cpid:"");
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannertopkey = "BannerAd"; //Define the banner placementID
        switch (tag)
        {
            case 1001:
            {
                UpltvBridge::setBannerShowCallback(bannertopkey, bannerCallback);
            }
                break;
        }
   }
}
```
### 2. 상단 배너 광고 실행하기

매개변수 `Placement ID`에 따른 스크린의 상단에 배너가 실행됩니다.

```cpp
/**
* @param cpPlaceId palcementid
*/
static void showBannerAdAtTop(const char*cpPlaceId);
```
샘플：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannertopkey = "BannerAd"; //배너 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
               UpltvBridge::showBannerAdAtTop(bannertopkey);
            }
                break;
        }
   }
}
```

> **상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.**

```cpp
/**
* 상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.
* @param padding: 상단 배너의 상쇄 값, 예를 들어 32를 입력하면 32 픽셀만큼 아래로 이동합니다.
* 이 기능은 Android 플랫폼에서는 지원되지 않습니다.
* 3002 버전부터 지원하고 있습니다.
*/
static void setTopBannerPadingForIphonex(int padding);
```

### 3. 상단 배너 광고 숨기기

```cpp
static void hideBannerAdAtTop();
```

예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannertopkey = "BannerAd"; //배너 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
              UpltvBridge::hideBannerAdAtTop();
            }
                break;
        }
   }
}
```
### 4. 하단 배너 광고 실행하기

매개변수 `Placement ID`에 따른 스크린의 하단 배너가 실행됩니다.

```cpp
/**
* @param cpPlaceId
*/
static void showBannerAdAtBottom(const char*cpPlaceId);
```
예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannerbottomkey = "BannerAd"; //배너 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
              UpltvBridge::showBannerAdAtBottom(bannerbottomkey);
            }
                break;
        }
   }
}
```
### 5. 하단 배너 광고 숨기기

```cpp
static void hideBannerAdAtBottom();
```
예시：
```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannerbottomkey = "BannerAd"; //배너 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
              UpltvBridge::hideBannerAdAtBottom();
            }
                break;
        }
   }
}
```
### 6. 배너 광고 삭제하기

```cpp
/**
* @param cpPlaceId banner placementID
*/
static void removeBannerAdAt(const char*cpPlaceId);
```
예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        const char* bannerbottomkey = "BannerAd"; //배너 placementID를 정의합니다.
        switch (tag)
        {
            case 1001:
            {
              UpltvBridge::removeBannerAdAt(bannerbottomkey);
            }
                break;
        }
   }
}
```

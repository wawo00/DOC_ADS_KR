## 리워드 동영상 광고(Reward video Ad)

#### 1. 리워드 동영상 광고 콜백 로드하기

  이 API는 현재 진행 중인 리워드 동영상 광고의 로딩 결과를 수신하는 데 사용됩니다. 이 인터페이스가 호출되면 <br />
  내부가 자동으로 릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.

```cpp
/**
* 리워드 동영상 광고 로드 콜백 인터페이스 메소드를 설정합니다.
* @param successCall 동영상이 성공적으로 로드됐을 때 콜백을 활성화 합니다.(cpadid, msg)
* @param failCall    동영상이 성공적으로 로드되지못했을 때 콜백을 활성화 합니다.(cpadid, msg)
*/
static void setRewardVideoLoadCallback(UpltvSdkStringCallback_2 successCall, UpltvSdkStringCallback_2 failCall);
```

예시：

```cpp
// 이 메소드는 광고가 성공적으로 로드되었을 때 호출 됩니다.
void rdLoadCallSuccess(const char *cpid, const char *msg) {
    log("====> reward video didLoad Success at cpid: %s", (cpid != nullptr ? cpid : ""));
}

// 이 메소드는 광고가 성공적으로 로드되지 못했을 때 호출 됩니다.
void rdLoadCallFail(const char *cpid, const char *msg) {
    log("====> reward video didLoad Fail at cpid: %s", (cpid != nullptr ? cpid : ""));
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                // 리워드 동영상 로드 콜백 인터페이스를 설정합니다.
                UpltvSdkStringCallback_2 loadSuccess = rdLoadCallSuccess;
                UpltvSdkStringCallback_2 loadFail = rdLoadCallFail;
                UpltvBridge::setRewardVideoLoadCallback(loadSuccess, loadFail);
            }
                break;
        }
   }
}
```

#### 2. 리워드 동영상 광고의 실행 콜백

리워드 동영상 광고의 이벤트(클릭, 닫기, 보상 등) 콜백을 수신하도록 콜백 인터페이스를 설정합니다. <br />
리워드 동영상은 콜백 인터페이스에 대한 참조가 내부적으로 저장되고 릴리스되지 않으므로 한 번만 설정하면 됩니다.

```cpp
/**
* @param callback(type, cpid)
*/
static void setRewardVideoShowCallback(UpltvSdkStringCallback_1 callback);
```

예시：

```cpp
// 동영상이 재생되는 동안 이벤트 유형에 따른 메소드를 콜백하여 수신합니다.
void rdShowCallback(UpltvAdEventEnum::AdEventType type, const char *cpid) {
    string s = "unkown";
    switch (type) {
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_SHOW:{
            s = "Did_Show";
        }
            break;
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_CLICK:{
            s = "Did_Click";
        }
            break;
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_CLOSE:{
            s = "Did_Close";
        }
            break;
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_GIVEN_REWARD:{
            s = "Did_Give";
        }
            break;
        case UpltvAdEventEnum::AdEventType::VIDEO_EVENT_DID_ABANDON_REWARD:{
            s = "Did_Abandon";
        }
            break;
    }
    log("====> reward video showCall event: %s, at cpid: %s", (s.c_str() != nullptr ? s.c_str() : ""), cpid?cpid:"");

}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                {
                //동영상 광고가 실행되는 동안 이벤트(클릭, 닫기, 보상 등) 콜백을 수신하도록 콜백 인터페이스를 설정합니다.
                UpltvBridge::setRewardVideoShowCallback(rdShowCallback);
            }
                break;
        }
   }
}
```

#### 3. 리워드 동영상 광고 준비 여부 확인하기

Bool 결과를 동시에 반환합니다. True는 광고가 디스플레이 준비가 되었다는 것을 의미하고, <br />
False는 광고가 실행되지 않고, 여전히 요청상태에 있다는 것을 의미합니다.

```cpp
 /**
* 이 메소드는 showRewardVideo(cpPlaceId) 전에 호출합니다.
* @return bool
*/
static bool isRewardReady();
```

예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                {
                // 동영상이 준비되었는지 판단합니다.
                bool r = UpltvBridge::isRewardReady();
                log("====> cpp print isRewardReady : %s",(r?"true":"false"));
            }
                break;
        }
   }
}
```
#### 4. 리워드 동영상 광고 실행하기

리워드 동영상 광고를 실행 할 때, cpPlaceld를 업로드 해야합니다. cpPlaceld는 비즈니스 관리와 같은 <br />
수익 소스를 구분하기 위한 매개변수입니다.

```cpp
/**
* @param cpPlaceId placementid
*/
static void showRewardVideo(const char* cpPlaceId);

```

예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                {
               if (UpltvBridge::isRewardReady()) {
               UpltvBridge::showRewardVideo(nullptr);
                }
            }
                break;
        }
   }
}
```

#### 5. 리워드 동영상 광고의 디버그 페이지

개발자가 광고의 사용 및 로딩 상태를 볼 수 있도록 하기 위해, UPLTV는 스크린 광고에 대한 디버그 페이지를 제공합니다. <br />
광고의 매개변수 및 로딩 상태 구조를 확인하실 수 있습니다.

```cpp
static void showRewardVideoDebugUI();
```

예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                {
                  UpltvBridge::showRewardVideoDebugUI();
                }
            }
                break;
        }
   }
}
```

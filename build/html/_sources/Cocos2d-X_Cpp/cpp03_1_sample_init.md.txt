## SDK 초기화

이 부분에서는 Xcode를 사용한 예시만 설명드리고 있습니다. <br />
다른 개발 툴을 사용하고 계신 분들께 불편을 끼쳐 드려 죄송합니다.

UPLTV SDK를 통해 간단히 초기화 하실 수 있습니다.

### 아래의 참조를 프로젝트에 추가합니다.

```cpp
#include "UpltvBridge.h"
```

### UPSDK 초기화

#### 1. 지역 매개변수를 기반으로 UPSDK 초기화하기

```cpp
/**
* Initial SDK
*/
static void initSdkByZone(int zone);
```
예시：

```cpp
/**
* 다른 API 인터페이스의 SDK를 사용하기 전에 SDK 초기화를 완료합니다.
* @param zone product distribution area, 0: 중국을 제외한 해외, 1: 중국, 2: IP에 의해 자동 설정
*/
UpltvBridge::initSdkByZone(0);
```

#### 2. 영역 매개변수와 콜백 기능을 기반으로 UPSDK 초기화하기

초기화 결과를 알고 싶으시면 아래의 메소드를 사용하시기 바랍니다.

```cpp
/**
* @param callback SDK 초기화가 완료된 후 콜백 인터페이스, 콜백 인터페이스는 Boolean 매개변수 콜백(Boolean)을 포함하며, 성공 및 실패를 나타냅니다.
*/
initSdkByZoneWithCall(int zone, UpltvSdkBoolCallback callback);
```
예시：

```cpp
/**
* The SDK가 콜백 인터페이스를 초기화를 진행 한 후, 매개변수 값 true로 초기화 성공을 나타내고, 그 외의 값으로 실패를 나타냅니다.
*/
void sdkCallback(bool r)
{
   log("====> cpp initSdkCallback(%s)", (r ? "true" : "false"));
}

void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        switch (tag)
        {
            case 1001:
            {
                //Initialize the SDK and listen for the callback results
                UpltvSdkBoolCallback callback = sdkCallback;
                UpltvBridge::initSdkByZoneWithCall(0, callback);
            }
                break;

        }
   }
}

```

#### 3. setCustomerId()

Android 플랫폼에서만 지원합니다. GP가 아닌 패키지일 경우, 부가적인 통계적 오류를 방지하기 위해 <br />
androidid로 전달할 수 있습니다.

```asp
/**
* 초기화하기 전에 이 메소드를 호출합니다.
* 3004(하위버전 5) 및 상위 버전에서만 이 메소드를 지원하고 있습니다.
* @param androidid
*/
UpltvBridge::setCustomerId(androidid);
```

#### 4. onApplicationFocus() 추가하기

다음 코드를 참조하여 현재 프로젝트의 AppDelegate 클래스에 onApplicationFocus() 메소드를 추가하여 <br />
UPSDK 게임 포그라운드에 변경 사항을 알립니다.

```cpp
void AppDelegate::applicationDidEnterBackground() {
    // 백그라운드에 진입할 때, UpltvBridge::onApplicationFocus(false)를 호출합니다.
    UpltvBridge::onApplicationFocus(false);

    Director::getInstance()->stopAnimation();
#if USE_AUDIO_ENGINE
    AudioEngine::pauseAll();
#elif USE_SIMPLE_AUDIO_ENGINE
    SimpleAudioEngine::getInstance()->pauseBackgroundMusic();
    SimpleAudioEngine::getInstance()->pauseAllEffects();
#endif
}

// 앱이 다시 실행되면 이 기능이 호출됩니다.
void AppDelegate::applicationWillEnterForeground() {
    // 포그라운드에 진입할 때, UpltvBridge::onApplicationFocus(true)를 호출합니다.
    UpltvBridge::onApplicationFocus(true);

    Director::getInstance()->startAnimation();
#if USE_AUDIO_ENGINE
    AudioEngine::resumeAll();
#elif USE_SIMPLE_AUDIO_ENGINE
    SimpleAudioEngine::getInstance()->resumeBackgroundMusic();
    SimpleAudioEngine::getInstance()->resumeAllEffects();
#endif
}
```

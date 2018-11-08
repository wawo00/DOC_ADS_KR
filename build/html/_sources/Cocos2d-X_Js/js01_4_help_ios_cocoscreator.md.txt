## iOS CocosCreator

  cocosCreator 1.7 및 상위 버전을 이용하셔서 액세스 하신다면 UpAdsBrigeJs.mm 내용을 바꿔야 합니다.
### 1. SeApi.h에 참조 추가하기

```objective-c
include "cocos/scripting/js-bindings/jswrapper/SeApi.h"
```

### 2. ScriptingCore를 se::ScriptEngine으로 바꾸기
`[UpAdsBrigeJs vokeMethod:arg1:arg2]`의 메소드에서 <br />
`ScriptingCore::getInstance()->evalString(jsCallStr.c_str());`를 <br />
`se::ScriptEngine::getInstance()->evalString( jsCallStr.c_str());`로 바꿉니다.

샘플：
```objective-c
+ (void)vokeMethod:(NSString*) name arg1:(NSString*) param1 arg2:(NSString*)param2 {
    //...
    if (jsCallStr.c_str()) {
        //ScriptingCore::getInstance()->evalString(jsCallStr.c_str());
        se::ScriptEngine::getInstance()->evalString(jsCallStr.c_str());
    }
}

```

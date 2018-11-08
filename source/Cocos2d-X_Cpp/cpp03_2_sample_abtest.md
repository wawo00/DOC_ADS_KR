## A/B 테스트

### 1. A/B 테스트 초기화 및 데이터 가져오기

SDK를 초기화한 후에 A/B 테스트를 진행하시고, A/B 테스트 초기화를 완료하려면 이 메소드를 호출합니다.

```cpp
/**
* @param gameAccountId          const char* 유형, 게임 유저의 id (필수사항)
* @param completeTask           bool 유형, 게임에서 튜토리얼 완수 여부
* @param isPaid                 int 유형, 결제 유저 판단, 0이면 비결제 유저
* @param promotionChannelName   const char* 유형, 광고 채널, 없으면 "" 전송 가능 
* @param gender                 const char* 유형, 성별 "M", "F"，모르면 "" 전송가능
* @param age                    int 유형, 연령, 모르면 -1 전송가능
* @param tags                   vector 유형, 태그, 없으면 null 전송 가능
*/
static void initAbtConfigJson(const char* gameAccountId, bool completeTask, int isPaid,const char* promotionChannelName, const char* gender, int age, vector<string> *tags);
```
예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        switch (tag)
        {
            case 1001:
            {
                // 아래 매개변수는 API 호출하는 것에 대한 설명에만 사용되며, 실제 구현 기능은 없습니다.
                vector<string> tags = {"This is the first element.","The second one.","The last one."};
                UpltvBridge::initAbtConfigJson("u89731", true, 0, "Facebook", "", -1, &tags);
            }
                break;
        }
   }
}
```
### 2. A/B 테스트 결과 가져오기

A/B 테스트를 초기화한 후 이 메소드를 통해 결과를 가져올 수 있습니다.

```cpp
/**
* 가져온 결과가 올바른지 확인하기 위해 initAbtConfigJson() 콜타임을 2초 이상 딜레이 하기를 권장합니다.
* @param cpPlaceId  string 유형
* @return const char*
*/
static const char* getAbtConfig(const char* cpPlaceId);
```
예시：

```cpp
void HelloWorld::touchEvent(cocos2d::Ref *pSender, cocos2d::ui::Widget::TouchEventType type, int tag)
{
    if (type == cocos2d::ui::Widget::TouchEventType::ENDED) {
        log("===> cpp button touch tag :%d",tag);
        switch (tag)
        {
            case 1001:
            {
                // config 구성을 가져옵니다.
                string r = UpltvBridge::getAbtConfig("pass");
                log("===> cpp getABtConfig r :%s",r.c_str());
            }
                break;
        }
   }
}
```

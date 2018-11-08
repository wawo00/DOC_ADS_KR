## A/B 테스트

### 1. A/B 테스트 초기화 및 데이터 가져오기

SDK를 초기화한 후에 A/B 테스트를 진행하시고, A/B 테스트 초기화를 완료하려면 이 메소드를 호출합니다.

```javascript

/** SDK 초기화 이후에 이 메소드를 호출합니다.
* @param gameAccountId          string 유형, 게임 유저의 id (필수사항)
* @param completeTask           bool 유형, 게임에서 튜토리얼 완수 여부
* @param isPaid                 number 유형, 결제유저 판단, 0이면 비결제 유저
* @param promotionChannelName   string 유형, 광고 채널, 없으면 "" 전송 가능 
* @param gender                 string 유형, 성별 "M", "F"，모르면 " " 전송가능
* @param age                    number 유형, 연령, 모르면 -1 전송가능
* @param tags                   String array 유형, 태그, 없으면 null 전송 가능
*/
initAbtConfigJson : function(gameAccountId, isCompleteTask, isPaid, promotionChannelName, gender, age, tags)
```

샘플：
```javascript
var initAbButton = this.createButton(x, y, "initABTest");
initAbButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.initAbtConfigJson("u89731", true, 0, "Facebook", "M", -1, ["This is the first element.", "The second one.", "The last one."]);
    }
}, this);
```

### 2. A/B 테스트 결과 가져오기

A/B 테스트를 초기화한 후 이 메소드를 통해 결과를 가져올 수 있습니다.

```javascript
/**
* 가져온 결과가 올바른지 확인하기 위해 initAbtConfigJson() 콜타임을 2초 이상 딜레이 하기를 권장합니다.
* @param cpPlaceId  string 유형
* @return const char*
*/
getAbtConfig : function(cpPlaceId)
```

샘플:
```javascript
var getAbButton = this.createButton(x, y, "getABConfig");
getAbButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var r = upltv.getAbtConfig("pass");
    }
}, this);
```
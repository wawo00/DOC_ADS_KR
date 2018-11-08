## 리워드 동영상 광고(Rewarded Video Ad)

#### 1. 리워드 동영상 광고 콜백 로드하기

이 API는 현재 진행 중인 리워드 동영상 광고의 로딩 결과를 수신하는 데 사용됩니다. 이 인터페이스가 호출되면 내부가 <br />
자동으로 릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.

```javascript
//@param loadsuccess  동영상이 성공적으로 로드됐을 때 콜백을 활성화 합니다.(cpadid, msg)
//@param failCall    동영상이 성공적으로 로드되지 못했을 때 콜백을 활성화 합니다.(cpadid, msg)
setRewardVideoLoadCallback : function(loadsuccess, locadfail)
```

샘플：
```javascript
var rdLoadCallButton = this.createButton(x, y, "rdLoadCall");
rdLoadCallButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.setRewardVideoLoadCallback(function(cpid, msg) {
            cc.log("===> js RewardVideo LoadCallback Success at: %s", cpid);
        }, function(cpid, msg) {
            cc.log("===> js RewardVideo LoadCallback Fail at: %s", cpid);
        });
    }
}, this);
```

#### 2. 리워드 동영상 광고의 실행 콜백
리워드 동영상 광고의 이벤트(클릭, 닫기, 보상 등) 콜백을 수신하도록 콜백 인터페이스를 설정합니다. <br />
리워드 동영상은 콜백 인터페이스에 대한 참조가 내부적으로 저장되고 릴리스되지 않으므로 한 번만 설정하면 됩니다.

```javascript

setRewardVideoShowCallback : function(showCall)
```

샘플：
```javascript
var rdShowCallButton = this.createButton(x, y, "rdShowCall");
rdShowCallButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.setRewardVideoShowCallback(function(type, cpid) {
            var event = "unkown";
            if (type == upltv.AdEventType.VIDEO_EVENT_DID_SHOW) {
                event = "Did_Show";
            }
            else if (type == upltv.AdEventType.VIDEO_EVENT_DID_CLICK) {
                event = "Did_Click";
            }
            else if (type == upltv.AdEventType.VIDEO_EVENT_DID_CLOSE) {
                event = "Did_Close";
            }else if (type == upltv.AdEventType.VIDEO_EVENT_DID_GIVEN_REWARD) {
                event = "Did_Given_Reward";
            }else if (type == upltv.AdEventType.VIDEO_EVENT_DID_ABANDON_REWARD) {
                event = "Did_Abandon_Reward";
            }
        });
    }
}, this);
```

#### 3. 리워드 동영상 광고 준비 여부 확인하기

Boolean 결과를 동시에 반환합니다. True는 광고가 디스플레이 준비가 되었다는 것을 의미하고, False는 광고가 <br />
실행되지 않고, 여전히 요청 상태에 있다는 것을 의미합니다.

```javascript
isRewardReady : function()
```

샘플：
```javascript
var readyRdUIButton = this.createButton(x, y, "rdReady");
readyRdUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var r = upltv.isRewardReady();
        cc.log("===> js isRewardReady r: %s", r.toString());
    }
}, this);
```

#### 4. 리워드 동영상 광고 실행하기

리워드 동영상 광고를 실행 할 때, cpPlaceld를 업로드 해야합니다. cpPlaceld는 비즈니스 관리와 같은 <br />
수익 소스를 구분하기 위한 매개변수입니다.

```javascript
// @param cpPlaceId placementid
showRewardVideo : function(cpPlaceId)
```

샘플：
```javascript
var showRdUIButton = this.createButton(x, y, "rdShow");
showRdUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var r = upltv.isRewardReady();
        if (r == true) {
            upltv.showRewardVideo();
        }
    }
}, this);
```

#### 5. 리워드 동영상 광고의 디버그 페이지

개발자가 광고의 사용 및 로딩 상태를 볼 수 있도록 하기 위해, UPLTV는 스크린 광고에 대한 디버그 페이지를 제공합니다. <br />
광고의 매개변수 및 로딩 상태 구조를 확인하실 수 있습니다.

```javascript
showRewardDebugUI : function()
```

샘플：
```javascript
var showRdUIButton = this.createButton(x, y, "rdDebugUI");
showRdUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.showRewardDebugUI();
    }
}, this);
```

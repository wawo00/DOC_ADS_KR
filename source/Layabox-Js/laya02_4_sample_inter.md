## 삽입 광고(Interstitial Ad) API

### 1. 삽입 광고 로드 콜백 설정하기

현재 삽입 광고를 수신하기 위해 성공 또는 실패 결과를 로드합니다. 이 인터페이스는 콜백이 발생하면 자동으로 <br />
릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.

```javascript
// @param cpPlaceId   placement ID
// @param loadSuccess 동영상이 성공적으로 로드됐을 때 콜백을 활성화 합니다.(cpadid, msg)
// @param loadFail    동영상이 성공적으로 로드되지 못했을 때 콜백을 활성화 합니다.(cpid, message)
//이 인터페이스가 호출되면 내부가 자동으로 릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.
setInterstitialLoadCallback : function(cpPlaceId, loadsuccess, locadfail)
```

샘플：
```javascript
var ilLoadCallUIButton = this.createButton(x, y, "ilLoadCall");
ilLoadCallUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.setInterstitialLoadCallback(cpPlaceId,
            function(cpid, msg) {
                cc.log("===> js il load callback success: %s at placementid:%s", msg, cpid);
            },
            function(cpid, msg) {
                cc.log("===> js il load callback fail: %s at placementid:%s", msg, cpid);
            });
    }
}, this);
```

### 2. 삽입 광고 실행 콜백 설정하기

콜백의 실행을 수신할 수 있는 삽입 광고의 디스플레이를 위해 콜백 인터페이스를 설정합니다. <br />
삽입 광고는 콜백 인터페이스에 대한 참조가 저장되고 릴리즈되지 않음을 나타냅니다.

```javascript
/**
* @param cpPlaceId placementid
* @param showCall  callback
*/
setInterstitialShowCallback : function(cpPlaceId, showCall)
```
샘플:
```javascript
var ilShowCallUIButton = this.createButton(x, y, "ilShowCall");
ilShowCallUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.setInterstitialShowCallback(cpPlaceId,
            function(type, cpid) {
                var event = "unkown";
                if (type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_SHOW) {
                    event = "Did_Show";
                }
                else if (type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_CLICK) {
                    event = "Did_Click";
                }
                else if (type == upltv.AdEventType.INTERSTITIAL_EVENT_DID_CLOSE) {
                    event = "Did_Close";
                }
            }
        );
    }
}, this);
```

### 3. 삽입 광고 준비 여부 확인하기

삽입 광고의 준비 여부는 매개변수 placementid에 의해 판단되고, Boolean 결과를 동시에 반환합니다. True는 광고가 <br />
디스플레이 준비가 되었다는 것을 의미하고, False는 광고가 실행되지 않고, 여전히 요청상태에 있다는 것을 의미합니다.

```javascript
// @param cpPlaceId  cpPlaceId
isInterstitialReady : function(cpPlaceId)
```

샘플:
```javascript
var ilReadyAsynUIButton = this.createButton(x, y, "ilReadyAsyn");
ilReadyAsynUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.isInterstitialReadyAsyn(cpPlaceId, function(r) {
            cc.log("===> js il ad isreadyasyn: %s at placementid:%s", r, cpPlaceId);
        });
    }
}, this);
```

### 4. 삽입 광고 실행하기

매개변수 `Placement ID`에 따른 삽입 광고가 실행됩니다.

```javascript
// @param cpPlaceId placementid
showInterstitialAd : function(cpPlaceId)
```

샘플：
```javascript
var ilShowUIButton = this.createButton(x, y, "ilShow");
ilShowUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var r = upltv.isInterstitialReady(cpPlaceId);
        if (r == true) {
            upltv.showInterstitialAd(cpPlaceId);
        }
    }
}, this);
```

### 5. 삽입 광고의 디버그 페이지

개발자가 광고의 사용 및 로딩 상태를 볼 수 있도록 하기 위해, UPLTV는 스크린 광고에 대한 디버그 페이지를 제공합니다. <br />
광고의 매개변수 및 로딩 상태 구조를 확인하실 수 있습니다.

```javascript
showInterstitialDebugUI : function()
```
샘플：
```javascript
var ilShowUIButton = this.createButton(x, y, "ilShowUI");
ilShowUIButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.showInterstitialDebugUI();
    }
}, this);
```

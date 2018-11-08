## 배너 광고(Banner Ad)

배너 광고(Banner Ad)는 상단 배너와 하단 배너로 구분되며, Javascrpit Plugin은 실행, 숨기기, 제거와 같은 이벤트 콜백 <br />
및 인터페이스를 제공하여 배너 광고의 구현을 더욱 단순화 합니다.

### 1. 배너 콜백

배너 광고는배너 광고 및 클릭, 이벤트 콜백 인터페이스의 디스플레이를 설정해야 합니다. 콜백 인터페이스는 Plug-in에 <br />
의해 내부적으로 저장되므로 여러 번 설정할 필요가 없습니다. upltv:removeBannerAdAt(cpPlaceId)라는 <br />
호출만 삭제됩니다.

```cpp

/**
* @param cpPlaceId  placementID
* @param callback
*/
setBannerShowCallback : function(cpPlaceId, bannerCall)
```
샘플：

```cpp
var bnCallButton = this.createButton(x, y, "SetBNCall");
bnCallButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        var bannerCall = function(type, cpadid) {
            var event = "unkown";
            if (type == upltv.AdEventType.BANNER_EVENT_DID_SHOW) {
                event = "Did_Show";
            }
            else if (type == upltv.AdEventType.BANNER_EVENT_DID_CLICK) {
                event = "Did_Click";
            }
            else if (type == upltv.AdEventType.BANNER_EVENT_DID_REMOVED) {
                event = "Did_Removed";
            }
        };

        upltv.setBannerShowCallback(topCpId, bannerCall);
        upltv.setBannerShowCallback(bottomCpId, bannerCall);
    }
}, this);
```

### 2. 상단 배너 광고 실행하기

매개변수 `Placement ID`에 따른 스크린의 상단에 배너가 실행됩니다.

```cpp
/**
* @param cpPlaceId palcementid
*/
showBannerAdAtTop : function(cpPlaceId)
```

샘플：

```cpp
var bnTopButton = this.createButton(x, y, "TopBNShow");
bnTopButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.showBannerAdAtTop(topCpId);
    }
}, this);
```

**상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.**

```cpp
/**
* 상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.
* @param padding: 상단 배너의 상쇄 값, 예를 들어 32를 입력하면 32 픽셀만큼 아래로 이동합니다.
* 이 기능은 Android 플랫폼에서는 지원되지 않습니다.
* 3002 버전부터 지원하고 있습니다.
*/
setTopBannerPadingForIphoneX : function(padding);
```

### 3. 상단 배너 광고 숨기기

```cpp
hideBannerAdAtTop : function()
```

샘플:
```javascript
var bnHideButton = this.createButton(x, y, "hideAllBN");
bnHideButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.hideBannerAdAtTop();
    }
}, this);
```

### 4. 하단 배너 광고 실행하기

매개변수 `Placement ID`에 의해 스크린의 하단 배너가 실행됩니다.

```javascript
// @param cpPlaceId placementid
showBannerAdAtBottom : function(cpPlaceId)
```

샘플:
```javascript
var bnBottomButton = this.createButton(x, y, "BottomBNShow");
bnBottomButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.showBannerAdAtBottom(bottomCpId);
    }
}, this);
```

### 5. 하단 배너 광고 숨기기

```javascript
hideBannerAdAtBottom : function()
```

샘플:
```javascript
var bnHideButton = this.createButton(x, y, "hideAllBN");
bnHideButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.hideBannerAdAtBottom();
    }
}, this);
```

### 6. 배너 광고 제거하기

`placementid`에 따른 배너를 제거할 수 있습니다.

```javascript
// @param cpPlaceId placementid
removeBannerAdAt : function(cpPlaceId)
```

샘플：
```javascript
var bnRemoveButton = this.createButton(x, y, "removeAllBn");
bnRemoveButton.addTouchEventListener(function(sender, type) {
    if (type == 2) {
        upltv.removeBannerAdAt(topCpId);
        upltv.removeBannerAdAt(bottomCpId);
    }
}, this);
```

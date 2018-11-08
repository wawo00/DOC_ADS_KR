## 배너 광고(Banner Ad)

배너 광고(Banner Ad)는 상단 배너와 하단 배너로 구분되며, TypeScriptPlugin은 실행, 숨기기, 제거와 같은 이벤트 콜백 <br />
및 인터페이스를 제공하여 배너 광고의 구현을 더욱 단순화 합니다.

### 1. 배너 콜백

배너 광고는 배너 광고 및 클릭, 이벤트 콜백 인터페이스의 디스플레이를 설정해야 합니다. 콜백 인터페이스는 <br />
 Plugin에 의해 내부적으로 저장되므로 여러 번 설정할 필요가 없습니다. <br />
upltv:removeBannerAdAt(cpPlaceId)라는 호출만 삭제됩니다.


```javascript
//광고 실행되는 콜백
    static bannerAdDidShow(callback:(cpPlaceId:string)=>void)
//광고 클릭되는 콜백
    static bannerAdDidClick(callback:(cpPlaceId:string)=>void)
//광고 제거되는 콜백
    static bannerAdDidRemove(callback:(cpPlaceId:string)=>void)
```

### 2. 상단 배너 광고 실행하기

매개변수 `Placement ID`에 따른 스크린의 상단에 배너가 실행됩니다.

```javascript
// @param cpPlaceId placementid
    static showBannerAdAtTop(cpPlaceId:string)
```

**상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.**

```javascript
/**
* 상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 위치를 조정하여 해결할 수 있습니다.
* @param padding: 상단 배너의 상쇄 값, 예를 들어 32를 입력하면 32 픽셀만큼 아래로 이동합니다.
* 이 기능은 Android 플랫폼에서는 지원되지 않습니다.
* 3002 버전부터 지원하고 있습니다.
*/
static setTopBannerPading(padding)
```

### 3. 상단 배너 광고 숨기기

```javascript
// @param cpPlaceId placementid
    static hideBannerAdAtTop()
```

### 4. 하단 배너 광고 실행하기

```javascript
// @param cpPlaceId placementid
    static showBannerAdAtBottom(cpPlaceId:string)
```

### 5. 하단 배너 광고 숨기기

```javascript
    static hideBannerAdAtBottom()
```

### 6. 배너 광고 제거하기
```javascript
// @param cpPlaceId placementid
    static removeBannerAdAt(cpPlaceId:string)
```

## 삽입 광고(Interstitial Ad) API

### 1. 삽입 광고 로드 콜백 설정하기

현재 삽입 광고를 수신하기 위해 성공 또는 실패 결과를 로드합니다. 이 인터페이스는 콜백이 발생하면 자동으로 <br />
릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.

```javascript
// 로딩의 결과를 가져옵니다.
//@param loadsuccess 동영상이 성공적으로 로드됐을 때 콜백을 활성화 합니다.(cpadid, msg)
//@param failCall    동영상이 성공적으로 로드되지 못했을 때 콜백을 활성화 합니다.(cpadid, msg)
    static getInterstitialAdLoadResult(cpPlaceId:string,
                                       success:(cpPlaceId:string,message:string)=>void,
                                       failure:(cpPlaceId:string,message:string)=>void)
```

샘플：

```javascript
upltv.getInterstitialAdLoadResult("ilLoadCall",function(cpid:string,msg:string){
//success
},function(cpid:string,msg:string){
//failure
});
```

### 2. 삽입 광고 콜백

콜백의 실행을 수신할 수 있는 삽입 광고의 디스플레이를 위해 콜백 인터페이스를 설정합니다. <br />
삽입 광고는 콜백 인터페이스에 대한 참조가 저장되고 릴리즈되지 않음을 나타냅니다.

```javascript
//실행 콜백
    static interstitialAdDidShow(callback:(cpPlaceId:string)=>void)
//클릭 콜백
    static interstitialAdDidClick(callback:(cpPlaceId:string)=>void)
//닫기 콜백
    static interstitialAdDidClose(callback:(cpPlaceId:string)=>void)
```

### 3. 삽입 광고 준비 여부 확인하기

삽입 광고의 준비 여부는 매개변수 placementid에 의해 판단되고, Boolean 결과를 동시에 반환합니다. True는 광고가 <br />
디스플레이 준비가 되었다는 것을 의미하고, False는 광고가 실행되지 않고, 여전히 요청 상태에 있다는 것을 의미합니다.

```javascript
// @param cpPlaceId placementid
// @param cpPlaceId callback,callback ,such as callback(true) or callback(false)
    static isInterstitialAdReady(cpPlaceId:string, callback:(ready:boolean)=>void)
```

### 4. 삽입 광고 실행하기

매개변수 `Placement ID`에 따른 삽입 광고가 실행됩니다.

```javascript
// @param cpPlaceId placementid
    static showInterstitialAd(cpPlaceId:string)
```

### 5. 삽입 광고의 디버그 페이지

개발자가 광고의 사용 및 로딩 상태를 볼 수 있도록 하기 위해, UPLTV는 스크린 광고에 대한 디버그 페이지를 제공합니다. <br />
광고의 매개변수 및 로딩 상태 구조를 확인하실 수 있습니다.

```javascript
// 디버그 베이지 열기
    static showInterstitialAdDebugUI()
```

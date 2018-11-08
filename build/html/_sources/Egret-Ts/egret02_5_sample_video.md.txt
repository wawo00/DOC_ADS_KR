## 리워드 동영상 광고(Rewarded Video Ad)

#### 1. 리워드 동영상 광고 콜백 로드하기

이 API는 현재 진행 중인 리워드 동영상 광고의 로딩 결과를 수신하는 데 사용됩니다. 이 인터페이스가 호출되면 <br />
내부가 자동으로 릴리즈되며 광고를 다시 수신할 때, 콜백 인터페이스를 재설정해야 합니다.

```javascript
// 로딩 결과를 가져옵니다.
//@param loadsuccess 동영상이 성공적으로 로드됐을 때 콜백을 활성화 합니다.(cpadid, msg)
//@param failCall    동영상이 성공적으로 로드되지 못했을 때 콜백을 활성화 합니다.(cpadid, msg)
    static getRewardAdLoadResult(success:(cpPlaceId:string,message:string)=>void,failure:(cpPlaceId:string,message:string)=>void)
```

샘플:
```javascript
upltv.getRewardAdLoadResult(function(cpid:string,msg:string){
//success
},function(cpid:string, msg:string){
//failure
});
```

#### 2. 리워드 동영상 광고의 실행 콜백
리워드 동영상 광고의 이벤트(클릭, 닫기, 보상 등) 콜백을 수신하도록 콜백 인터페이스를 설정합니다. <br />
리워드 동영상은 콜백 인터페이스에 대한 참조가 내부적으로 저장되고 릴리스되지 않으므로 한 번만 설정하면 됩니다.

```javascript
// 실행 콜백
// @param cpPlaceId placementid
    static rewardAdDidOpen(callback:(cpPlaceId:string)=>void)

// 클릭 콜백
// @param cpPlaceId placementid
    static rewardAdDidClick(callback:(cpPlaceId:string)=>void)

// 닫기 콜백
// @param cpPlaceId placementid
    static rewardAdDidClose(callback:(cpPlaceId:string)=>void)

// 보상 콜백
// @param cpPlaceId placementid
    static rewardAdDidGiven(callback:(cpPlaceId:string)=>void)

// 보상하지 않는 콜백
// @param cpPlaceId placementid
    static rewardAdDidAbandon(callback:(cpPlaceId:string)=>void)
```

#### 3. 리워드 동영상 광고 준비 여부 확인하기

Boolean 결과를 동시에 반환합니다. True는 광고가 디스플레이 준비가 되었다는 것을 의미하고, <br />
False는 광고가 실행되지 않고, 여전히 요청상태에 있다는 것을 의미합니다.

```javascript
// showRewardVideo(cpPlaceId)전에 이 메소드를 호출합니다.
    static isRewardAdReady(callback:(ready:boolean)=>void)
```

#### 4. 리워드 동영상 광고 실행하기

리워드 동영상 광고를 실행 할 때, cpPlaceld를 업로드 해야합니다. cpPlaceld는 비즈니스 관리와 같은 <br />
수익 소스를 구분하기 위한 매개변수입니다.

```javascript
// @param cpPlaceId placementid
    static showRewardAd(cpPlaceId:string)
```

#### 5. 리워드 동영상 광고의 디버그 페이지

개발자가 광고의 사용 및 로딩 상태를 볼 수 있도록 하기 위해, UPLTV는 스크린 광고에 대한 디버그 페이지를 제공합니다. <br />
광고의 매개변수 및 로딩 상태 구조를 확인하실 수 있습니다.

```javascript
showRewardDebugUI : function()
```

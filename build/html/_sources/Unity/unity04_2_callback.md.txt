
## PolyADSDK 콜백 인터페이스

### 1. 리워드 동영상 광고(Reward Video Ad) 콜백 인터페이스

#### API 참조

```csharp
public static Action <bool, string> UPSDKInitFinishedCallback = null;

/*
* 리워드 동영상 광고
* 리워드 동영상 광고가 실행될 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPRewardDidOpenCallback = null;

/*
* 리워드 동영상 광고가 클릭될 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPRewardDidClickCallback = null;

/*
* 리워드 동영상 광고가 닫힐 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPRewardDidCloseCallback = null;  

/*
* 보상 지급이 발생할 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPRewardDidGivenCallback = null;

/*
* 보상을 지급할 조건에 만족하지 않거나  지급하지 못한 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPRewardDidAbandonCallback = null;

```
### 콜백 인터페이스 순서

#### 1. UPRewardDidOpenCallback

리워드 동영상 광고가 디스플레이가 성공되면 호출됩니다.

#### 2. UPRewardDidClickCallback

리워드 동영상 광고를 클릭할 때만 호출됩니다.

#### 3. UPRewardDidGivenCallback/UPRewardDidAbandonCallback

광고가 실행되고, 보상 발생여부는 아래의 결과에 따라 결정됩니다. 보상이 발생했을 때 UPRewardDidGivenCallback이 <br />
호출됩니다. 보너스가 발생하지 않는다면, UPRewardDidAbandonCallback 가 호출됩니다.

#### 4. UPRewardDidCloseCallback

UPRewardDidCloseCallback은 리워드 동영상 광고가 닫힐 때 호출되며 마지막으로 실행되는 콜백입니다.

> 요약：`RewardDidOpen`->`RewardDidClick(if have)`->`RewardDidGiven(or RewardDidAbandon)`->`RewardDidClose`

### 2. 삽입 광고(Interstitial Ad)에 대한 콜백

#### API 참조

```csharp
/*
* 삽입 광고
* 삽입 광고가 실행될 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPInterstitialDidShowCallback = null;

/*
* 삽입 광고가 클릭될 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPInterstitialDidClickCallback = null;

/*
* 삽입 광고가 닫힐 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPInterstitialDidCloseCallback = null;

```

### 콜백 인터페이스 순서

#### 1. UPInterstitialDidShowCallback

삽입 광고의 디스플레이가 성공되면 호출됩니다.

#### 2. UPInterstitialDidClickCallback

게임 유저가 광고를 클릭할 때만 호출됩니다.

#### 3. UPInterstitialDidCloseCallback

UPInterstitialDidCloseCallback은 삽입 광고가 닫힐 때 호출되며 마지막으로 실행되는 콜백입니다.

> 요약：`InterstitialDidShow`->`InterstitialDidClick(if have)`->`InterstitialDidClose`

### 3. 배너 콜백:

UPBannerDidShowCallback은 배너가 처음 실행될 때만 호출되고 UPBannerDidClickCallback은 배너를 클릭할 때마다 <br />
호출됩니다. UPBannerDidRemoveCallback은 배너 광고를 제거하기 호출됩니다.

```csharp

/*
* 배너 광고
* 배너 광고가 실행될 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPBannerDidShowCallback = null;

/*
* 배너 광고가 클릭될 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPBannerDidClickCallback = null;

/*
* 광고가 삭제될 경우 콜백이 호출됩니다.
*/
public static Action <string, string> UPBannerDidRemoveCallback = null;



## Unity Plugin 광고 실행하기

#### 삽입 광고(Interstitial Ad) 실행하기
```csharp
public static void showIntersitialAd(string cpPlaceId);
```

> 매개 변수: cpPlaceId는 광고 위치를 나타냅니다. 올바르게 연동되었는지 확인하시기 바랍니다.

> 이 메소드는 cpPlaceId에 따라 해당 광고가 존재하는지 묻고, 존재하는 경우 광고가 준비되었는지 여부를 <br />
추가로 묻습니다. 삽입 광고는 둘 다 참인 경우에만 실행됩니다. <br />
즉, showIntersitialAd()가 isInterstitialReady()를 호출합니다.

#### 삽입 광고 상태 탐지하기
일반적으로 showIntersitialAd() 메소드를 호출하여 광고를 디스플레이 할 수 있습니다. <br />
삽입 광고의 상태에 따라 버튼을 표시 여부를 제어하려는 경우 이 메소드를 사용합니다.

    public static bool isInterstitialReady(string cpPlaceId)

샘플


"**inter_bbbb**"와 "**inter_ccc**"는 삽입 광고의 두 위치입니다.

```csharp
public void onBtnIntertitialClick()
{
    if (UPSDK.isInterstitialReady("inter_bbb")) {
        UPSDK.showIntersitialAd("inter_bbb");
    }
}

public void onBtnIntertitial_CCC_Click()
{
    if (UPSDK.isInterstitialReady("inter_ccc")) {
        UPSDK.showIntersitialAd("inter_ccc");
    }
}
```


#### 리워드 동영상 광고 실행하기

```csharp
public static void showRewardAd(string cpCustomId)
```

> 매개 변수: cpCustomId는 사용자 정의 식별자를 의미하며, showRewardAd를 호출할 때 <br />
광고가 준비되었는지도 확인합니다.


#### 리워드 동영상 광고 상태 탐지
일반적으로 showRewardAd() 메소드를 호출하여 리워드 동영상 광고를 디스플레이 할 수 있습니다. <br />
이 메소드는 isInterstitialReady()와 유사하며 특정 사항을 충족시키기 위해 제공됩니다.

```csharp
public static bool isRewardReady()
```

샘플:
```csharp
public void onBtnReward_aaa_Click()
{
    if (UPSDK.isRewardReady()) {
         UPSDK.showRewardAd("aaa");
    }
}
```

>매개 변수 "aaa"는 사용자 정의 식별자이며, 통계용으로 사용됩니다.

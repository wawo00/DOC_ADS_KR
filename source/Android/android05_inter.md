## 삽입 광고(Interstitial Ad)


### 광고 객체 초기화

아래의 코드를 사용하여 삽입 광고의 초기화 합니다.
> 매개변수 `Placement ID` 를 의미 있는 이름으로 설정할 수 있습니다. 정확한 판단이 어렵다면, <br />
UPLTV의 기술 지원팀 담당자와 상의하시기 바랍니다. UPLTV 는 각 `Placement ID`에서 발생한 수익을 <br />
제공하고 있으므로, 광고 노출 위치마다 다른 `Placement ID`를 사용해야 합니다. 가령, 게임의 정지 화면에서 <br />
UPSDK의 초기화 설정할 때는 "정지" 또는 "메뉴"를 사용할 수 있습니다.



    mInterstitialAd = new UPInterstitialAd(this, "Placement ID");

### 로드 콜백(Load Callback)
인터페이스 `load`를 사용하여 콜백 기능을 설정할 수 있습니다. <br />
이는 선택사항이며, 특별히 필요하지 않으면, 사용하지 않아도 됩니다.

```java
public void load(UPInterstitialLoadCallback callback)
```
샘플 코드:
```java
UPInterstitialAd mInterstitialAdAAA = new UPInterstitialAd(InterstitialActivity.this, "inter_aaa");
final UPInterstitialLoadCallback callback = new UPInterstitialLoadCallback() {
        @Override
        public void onLoadSuccessed(String placement) {
            Log.i(TAG, "InterstitialAd " + placement + " onLoadSuccessed:");

            if (placement.equals("inter_aaa")) {
                // load successed,could show
            }
        }

        @Override
        public void onLoadFailed(String placement) {
            Log.i(TAG, "InterstitialAd " + placement + " onLoadFailed:");
            if (placement.equals("inter_aaa")) {
                // load failed
            }
        }
    };

// set callback
mInterstitialAdAAA.load(callback);
```
### 액션 콜백(Action Callback)
인터페이스 `setUPInterstitialAdListener`를 사용하여 콜백 기능을 설정할 수 있습니다. <br />
이는 선택사항이며, 특별히 필요하지 않으면, 사용하지 않아도 됩니다.

    mInterstitialAd.setUPInterstitialAdListener(new UPInterstitialAdListener() {
        @Override
        public void onClicked() {
            // click callback
        }

        @Override
        public void onClosed() {
            // close callback
        }

        @Override
        public void onDisplayed() {
            // impression callback
        }
    });



### 삽입 광고 실행하기
`show` 메소드를 호출하여 게임에서 원할 때마다 광고를 노출할 수 있습니다.

**참고: `show` 메소드를 호출하기 전에 `isReady`를 호출하여 동영상이 준비되었는지 확인하시기바랍니다.**

    if (mInterstitialAd != null && mInterstitialAd.isReady()) {
        mInterstitialAd.show();
    }
### 디버그 페이지
동영상 광고의 디스플레이 및 로딩 상태를 쉽게 이해할 수 있도록, 디버그 페이지를 통해 동영상 광고의 작동 상태를 <br />
알 수 있도록 제공하고 있습니다.

아래의 코드를 사용하여 디버그 페이지를 조정할 수 있습니다.

    if (mInterstitialAd != null) {
        mInterstitialAd.showInterstitialDebugActivity(this);
    }

페이지 구성은 다음과 같습니다. (왼쪽에서 오른쪽으로)
- 광고 네트워크
- SDK 적용 여부 및 버전정보
- 서버 구성을 통한 광고 실행 여부
- 광고 로딩 성공 여부

광고가 성공적으로 로딩된 경우, 해당 항목을 클릭하면 광고가 실행됩니다.

![as_debug_inter_view1](http://docc.upltv.com/uploads/201810/5bcd4065a93a4_5bcd4065.png "as_debug_inter_view1")
![as_debug_inter_view2](http://docc.upltv.com/uploads/201810/5bcd408d13bcb_5bcd408d.png "as_debug_inter_view2")

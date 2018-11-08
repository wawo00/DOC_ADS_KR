## 리워드 동영상 광고(Rewarded Video Ad)

### 광고 객체 초기화 설정  

리워드 동영상은 아래와 같은 방법을 통해 초기화를 할 수 있습니다.

    mVideoAd = UPRewardVideoAd.getInstance(this);

### 리워드 동영상 광고 실행하기

리워드 동영상 광고를 노출하려면, `isReady`를 호출하여 동영상이 준비되었는지 확인해야 합니다. `isReady`의 리턴 값으로 <br />
"재생" 버튼 표시 상태를 정한 후, `show`메소드를 호출하여 동영상 광고를 표시합니다. 각각의 광고 노출 위치에 <br />
해당하는 올바른 매개변수 `Placement ID` 값을 사용하여 주시기 바랍니다.

**반드시 `show` 메소드를 호출하기 전에 `isReady`를 호출하여 동영상이 준비되었는지 확인하십시오.**

> 매개변수 `Placement ID` 를 의미 있는 이름으로 설정할 수 있습니다. 정확한 판단이 어렵다면, <br />
UPLTV의 기술 지원팀 담당자와 상의하시기 바랍니다. UPLTV 는 각 `Placement ID`에서 발생한 수익을 <br />
제공하고 있으므로, 광고 노출 위치마다 다른 `Placement ID`를 사용해야 합니다. 가령, 게임의 정지 화면에서 <br />
UPSDK의 초기화 설정할 때는 "정지" 또는 "메뉴"를 사용할 수 있습니다.



    if (mVideoAd != null && mVideoAd.isReady()) {
        mVideoAd.show("Ad Placement");
    }

### 콜백(Callback)

인터페이스 `setUPVideoAdListener`를 사용하여 콜백 기능을 설정할 수 있습니다. 보통 게임 유저에게 리워드를 지급할지에 <br />
대한 여부를 결정하는 메소드`onVideoAdReward`와`onVideoAdDontReward`를 확인하면 됩니다.

**참고: `onVideoAdReward`의 콜백을 받은 후 리워드를 지급하고, 다른 상황에서는 지급하지 않습니다.**

    mVideoAd.setUPVideoAdListener(new UPRewardVideoAdListener() {
        @Override
        public void onVideoAdClicked() {
            // click callback
        }

        @Override
        public void onVideoAdClosed() {
            // close callback
        }

        @Override
        public void onVideoAdDisplayed() {
            // impression callback
        }

        @Override
        public void onVideoAdReward() {
            // reward callback, means user already reached the reward condition
        }

        @Override
        public void onVideoAdDontReward(String reason) {
            // DON'T reward callback, means user hasn't reached the reward condition, like too short time to view video.
        }
    });

### 디버그 페이지

동영상 광고의 디스플레이 및 로딩 상태를 쉽게 이해할 수 있도록, 디버그 페이지를 통해 동영상 광고의 작동 상태를 <br />
알 수 있도록 제공하고 있습니다. 아래의 코드를 사용하여 디버그 페이지를 조정할 수 있습니다.

    if (mVideoAd != null) {
        mVideoAd.showVideoDebugActivity(this);
    }


페이지 구성은 아래와 같습니다(왼쪽에서 오른쪽으로).
- 광고 네트워크
- SDK 적용 여부 및 버전정보
- 서버 구성을 통한 광고 실행 여부
- 광고 로딩 성공 여부

광고가 성공적으로 로딩된 경우, 해당 항목을 클릭하면 광고가 실행됩니다.

![as_debug_video_view1](http://docc.upltv.com/uploads/201810/5bcd40ce08dbf_5bcd40ce.jpg "as_debug_video_view1")
![as_debug_video_view2](http://docc.upltv.com/uploads/201810/5bcd40de0f43d_5bcd40de.jpg "as_debug_video_view2")

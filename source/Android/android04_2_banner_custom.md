## 맞춤형 배너 광고(Banner Ad)
### 광고 객체 초기화 설정
"가로형 배너"와 "사각형" 레이아웃이 지원되오니, 제품에 적합한 스타일을 선택하세요.

> 매개변수 `Placement ID` 를 의미 있는 이름으로 설정할 수 있습니다. 정확한 판단이 어렵다면, <br />
UPLTV의 기술 지원팀 담당자와 상의하시기 바랍니다. UPLTV 는 각 `Placement ID`에서 발생한 수익을 <br />
제공하고 있으므로, 광고 노출 위치마다 다른 `Placement ID`를 사용해야 합니다. 가령, 게임의 정지 화면에서 <br />
UPSDK의 초기화 설정할 때는 "정지" 또는 "메뉴"를 사용할 수 있습니다.

"가로형 배너"의 경우, 아래의 코드를 사용하여 광고 객체의 초기화 설정합니다.

    mBannerAd = new UPBannerAd(this, "Your Placement ID");

"사각형 배너의 경우, 아래의 코드를 사용하여 초기화 설정합니다.

    mRectangleAd = new UPRectangleAd(this, "Your Placement ID");

### 배너 광고 실행하기
"가로형 배너" 광고를 게시한다고 가정했을 때, 먼저 아래와 같이 레이아웃 파일에 광고의 parent view를 추가해야 합니다.

    <LinearLayout
        android:id="@+id/banner_container"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"/>

그다음, `getBannerView()`를 호출하여 광고 보기를 불러온 후, parent view에 추가할 수 있습니다. <br />
광고가 성공적으로 로드되면, 배너가 노출됩니다.

    banner_container = (LinearLayout) findViewById(R.id.banner_container);
    banner_container.addView(mBannerAd.getBannerView());

### 콜백(Callback) 인터페이스
인터페이스`setUPBannerAdListener`를 사용하여 콜백 기능을 설정할 수 있습니다. <br />
이는 선택사항이며, 특별히 필요하지 않으면, 사용하지 않아도 됩니다.

    mBannerAd.setUPBannerAdListener(new UPBannerAdListener() {
        @Override
        public void onClicked() {
            // click callback
        }

        @Override
        public void onDisplayed() {
            // impression callback
        }
    });

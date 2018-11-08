## 배너 광고(Banner Ad) 빠른 설정

### UPGameEasyBannerWrapper를 적용한 화면

배너 광고를 프로젝트의 상단이나 하단에만 추가하길 원하며, 배너 광고의 액티비티가 현재 프로젝트의 <br />
액티비티인 경우, 아래의 권장 방법을 통해 더욱 빠르고 간편하게 결합할 수 있습니다. <br />
이 경우, UPSDK는 다음의 객체를 패키지로 제공하고 있습니다.
`com.up.ads.wrapper.banner.UPGameEasyBannerWrapper`

### UPGameEasyBannerWrapper API 인터페이스 소개

  UPGameEasyBannerWrapper는 가로형으로 설계되었으므로, <br />
  아래와 같이 `UPGameEasyBannerWrapper.getInstance().XXXX()` 메소드를 사용하여 API 인터페이스를 호출할 수 있습니다.

```java
   /**
   * 게임 배너의 초기화 위해 우선 이 메소드를 호출해야 합니다.
   * @param gameActivity는 남겨두면 안됩니다. 현재 배너 광고가 의존되는 액티비티입니다.
   */
   public void initGameBannerWithActivity(Activity gameActivity);

   /**
   * 광고 위치에 의해 배너 광고가 현재 액티비티의 상단에 실행됩니다.
   * BadTokenException: Unable to add window -- token null is not valid를 방지하기 위해 액티비티 onresume후 이 메소드를 호출합니다.
   * @param cpPlaceId banner ad placement는 몇몇 광고의 비즈니스 유형으로 사용됩니다.
   */
    public void showTopBannerAtADPlaceId(String cpPlaceId);

   /**
   * 광고 위치에 의해 배너 광고가 현재 액티비티의 하단에 실행됩니다.
   * BadTokenException: Unable to add window -- token null is not valid를 방지하기 위해 onresume후 이 메소드를 호출합니다.
   * @param cpPlaceId banner ad placement는 몇몇 광고의 비즈니스 유형으로 사용됩니다.
   */
   public void showBottomBannerAtADPlaceId(String cpPlaceId);

   /**
   * 광고 위치에 의해 배너의 콜백 프록시를 추가합니다.
   * @param cpPlaceId banner ad placement는 몇몇 광고의 비즈니스 유형으로 사용됩니다.
   * @param callback banner 콜백
   */
   public void addBannerCallbackAtADPlaceId(String cpPlaceId, UPBannerAdListener callback);

   /**
   * 해당 콜백을 삭제하지 않고도, 다른 광고 위치의 배너 광고를 제거 할 수 있습니다.
   * 만약 광고 위치가 실존하지 않는다면 어떠한 작용도 하지 않습니다.
   * 배너 광고가 제거 된 후, 다음 실행 시 다시 로드 됩니다.
   * hideTopBanner() 혹은 hideBottomBanner() 를 이용하여 현재 배너 광고를 제거할 수 있습니다.
   * @param cpPlaceId banner ad placement는 몇몇 광고의 비즈니스 유형으로 사용됩니다.
   */
   public void removeGameBannerAtADPlaceId(String cpPlaceId);

   /**
   * 현재 상단 배너 광고를 숨깁니다.
   * 광고 위치를 구분 할 필요가 없습니다.
   * 광고를 한 번 더 실행할 시, showTopBannerAtADPlaceId()를 호출하면 됩니다.
   * 2037 버전부터 지원합니다.
   */
    public void hideTopBanner();

   /**
   * 현재 하단 배너 광고를 숨깁니다.
   * 광고 위치를 구분 할 필요가 없습니다.
   * 광고를 한 번 더 실행할 시, showBottomBannerAtADPlaceId()를 호출하면 됩니다.
   *2037 버전부터 지원합니다.
   */
    public void hideBottomBanner();
```

### UPGameEasyBannerWrapper 사용 예시

아래의 방법을 통해 현재 액티비티 하단에 배너 광고를 간단하게 빌드할 수 있습니다.

```java

protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_banner);
        // 배너 초기화 합니다.
        UPGameEasyBannerWrapper.getInstance().initGameBannerWithActivity(this);
        // 콜백 인터페이스 추가합니다.
        UPGameEasyBannerWrapper.getInstance().addBannerCallbackAtADPlaceId("banner_aaa", new UPBannerAdListener() {
            @Override
            public void onClicked() {
                Log.i(TAG, "banner_aaa onClicked ");
            }

            @Override
            public void onDisplayed() {
                Log.i(TAG, "banner_aaa onDisplayed ");
            }
        });
        // 0.2초 딜레이 후에 광고 실행합니다. Demo는 activity onresume 이후에  showBottomBannerAtADPlaceId()를 호출합니다.
        (new Handler(Looper.getMainLooper())).postDelayed(new Runnable() {
            @Override
            public void run() {
                UPGameEasyBannerWrapper.getInstance().showBottomBannerAtADPlaceId("banner_aaa");
            }
        }, 200);
}
```

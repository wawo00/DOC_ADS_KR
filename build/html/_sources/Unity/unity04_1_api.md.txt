## API 디스크립션

### UPSDK Unity Plugin 사용

```csharp
using Polymer.PolyADSDK ;
```

> API와 콜백 인터페이스(액션)는 정적 메소드로 정의되어 다른 모델에 바로 액세스 될 수 있습니다.

### API 참조

```csharp

		/*
		* Plugin 버전 번호를 가져옵니다.
		*/
		public static string getVersionOfPlugin();

		/*
		* 현재 Plugin에 대해 지원되는 플랫폼의 버전 번호를 가져옵니다.
		 현재 지원되는 iOS 및  Android 버전을 가져옵니다. 런타임 시 유저 지정 환경에 따라 자동으로 매칭합니다.
		public static string getVersionOfPlatform();
		*/
		public static string getVersionOfPlatform();

		/*
		* SDK 초기화
		* 2030보다 하위 버전일 경우, 이 메소드로 초기화합니다.
		*/
		public static string initPolyAdSDK();

		 /*
		* UPSDK를 초기화합니다.
		* @param zone:0, 중국을 제외한 해외；1, 중국；2, IP에 의해 자동 설정
		* 2030보다 하위 버전일 경우, 이 메소드로 초기화합니다.
		*/
		public static string initPolyAdSDK(int zone);

		/*
		* UPSDK 광고를 위한 A/B테스트 구성을 초기화합니다.
        * @param gameAccountId        게임 유저의 id (필수사항)
        * @param completeTask         게임에서 튜토리얼 완수 여부
        * @param isPaid               결제유저 판단, 0이면 비결제 유저
        * @param promotionChannelName 광고 채널
        * @param gender               "M", "F"
        * @param age                  -1은 unknown
        * @param tags                 
		*/
		public static void initAbtConfigJson(string gameAccountId, bool completeTask, int isPaid, string promotionChannelName,  string gender, int age, string[] tags) ;

		/*
		* UPSDK 광고의 A/B테스트 구성을 가져옵니다.
		* Json 문자열 형식(character string)는 리턴된 결과인데 아마 null 일겁니다.
		* API 호출하기 전, initAbtConfigJson() 을 호출해서 A/B테스트 구성 초기화 완성합니다.
		*/
		public static string getAbtConfig(string placementId);

		/*
		 * 삽입 광고가 준비되었는지 확인합니다.
		 */
		public static bool isInterstitialReady(string cpPlaceId);

		/*
		* 리워드 동영상 광고가 준비되었는지 확인합니다.
		* UI 버튼 상태를 업데이트 할 때와 같은 상황에서 이 메소드를 지속적으로 호출하지 않고, setRewardVideoLoadCallback()를 콜백하여 수신합니다.
		*/
		public static bool isRewardReady();

		/*
		* 리워드 동영상 광고를 실행합니다. cpCustomId는 사용자 지정 표시이며, 필수 입력 사항입니다.
		*/
		public static void showRewardAd(string cpCustomId);

		/*
		* 삽입 광고를 실행합니다.
		* cpPlaceId는 사용자 지정 표시이며 올바르게 입력 되었는지 확인합니다.
		*/
		public static void showInterstitialAd(string cpPlaceId);

		/*
		* 스크린 상단에 배너 광고를 실행합니다.
		* cpPlaceId는 사용자 지정 표시이며 올바르게 입력 되었는지 확인합니다.
		*/
		public static void showBannerAdAtTop(string cpPlaceId);

		/*
		* 스크린 하단에 배너 광고를 실행합니다.
		*/
		public static void showBannerAdAtBottom(string cpPlaceId);

		/*
		 * 현재 상단 배너 광고를 숨깁니다.
		 * 광고 위치를 구분할 필요가 없습니다.
		 * 재실행 시, showBannerAdAtTop()를 호출해야 합니다.
		 */
		public static void hideBannerAdAtTop();

		/*
		 * 현재 하단 배너 광고를 숨깁니다.
		 * 광고 위치를 구분할 필요가 없습니다.
		 * 재실행 시, showBannerAdAtTop()를 호출해야 합니다.
		 * 2037 버전부터 지원하고 있습니다.
		 */
		public static void hideBannerAdAtBottom();

		/*
		* 배너 광고 삭제하기
		*/
		public static void removeBannerAdAt(string cpPlaceId);

		/*
		* Android 플랫폼의 manifest.xml 에서 정의한 packagename을 설정합니다.
		*/
		public static void setManifestPackageName(string packagename);

		/*
		* 광고 닫기 보여주기 (Android 플랫폼 에서만 가능)
		*/
		public static void onBackPressed();


		/*
		 * 리워드 동영상 광고 로딩을 위한 콜백 API를 추가합니다.
		 * @param success  로딩 성공의 콜백
		 * @param fail     로딩 실패의 콜백
		 *
		 * 콜백 매개변수의 유형: Action <string,string>
		 * T첫 번째는 cpPlaceID이며 광고의 placementid를 나타내며, 공백 혹은 null 입니다. 두 번째 매개변수는 정보 전달 기능이며, 공백 혹은 null 입니다.
		 * 2028 버전부터 지원하고 있습니다.
		 */
		public static void setRewardVideoLoadCallback(Action <string,string> success, Action <string, string> fail);

		/*
		 * 삽입 광고 로딩을 위한 콜백 API를 추가합니다.
		 * @param cpPlaceId:  삽입 광고의 표시이며, 공백 혹은 null이 되면 안됩니다.
		 * @param success  로딩 성공의 콜백
		 * @param fail t   로딩 실패의 콜백
		 *
		 * 콜백 매개변수의 유형: Action <string,string>
		 * 첫 번째는 cpPlaceID이며 광고의 placementid를 나타내며, 공백 혹은 null 입니다. 두 번째 매개변수는 정보 전달 기능이며, 공백 혹은 null 입니다.
		 * 2028 버전부터 지원하고 있습니다.
		 */
		public static void setIntersitialLoadCallback(string cpPlaceId, Action <string,string> success, Action <string, string> fail)

		/*
		 * 리워드 동영상 디버그 정보를 보여주는데 사용됩니다.
		 * 2028 버전부터 지원하고 있습니다.
		 */

		public static void showRewardDebugView();

		/*
		 * 삽입 광고 디버그 정보를 보여주는데 사용됩니다.
		 * 2028 버전부터 지원하고 있습니다.
		 */
		public static void showInterstitialDebugView();


         /**
         * SDK 초기화할 때, 자동 광고 로딩이 아닌 게임에 따라 맞춤형 설정을 통해 로딩을 하고자 하는 경우
         * 조건:  SDK가 임의로 자동 광고를 로딩을 할수 없거나 UPLTV의 백그라운드에서 이러한  기능을 꺼 두었을 경우.
         * 3002 버전부터 지원하고 있습니다.
         */
         public static void loadAvidlyAdsByManual() ;

         /**
         * 상단 배너가 아이폰X의 상태 표시줄에 의해 블럭되면 상단 배너의 값을 조정하여 해결할 수 있습니다.
         * @param padding: 상단 배너의 상쇄 값, 예를 들어 75를 입력하면 75 픽셀만큼 이래로 이동합니다.
         * 3002 버전부터 지원하고 있습니다.
         */
         public static void setTopBannerForIphonex(int padding);

         /**
          * 상단 배너가 화웨이 P20의 상태 표시줄에 의해 블럭되면 상단 배너의 값을 조정하여 해결할 수 있습니다.
          * @param padding: 상단 배너의 상쇄 값, 예를 들어 75를 입력하면 75 픽셀만큼 이래로 이동합니다.
          * 3004 버전부터 지원하고 있습니다.
          */
          public void setTopBannerForHuaWeiP20(int padding);

         /**
         * 데이터 수집 권한 승인 여부 창을 팝업 합니다.
         * @param callback
         * 3003 및 상위 버전에서만 지원하고 있습니다.
         */
         public static void notifyAccessPrivacyInfoStatus(Action <UPConstant.         UPAccessPrivacyInfoStatusEnum, string> callback);

         /**
         * GDPR 승인을 진행하고, 승인 결과를 UPSDK 초기화 전에 UPSDK에 알려줍니다.
         * @param enumValue
         * 3003 및 상위 버전에서만 지원하고 있습니다.
         */
         public static void updateAccessPrivacyInfoStatus(UPConstant.         UPAccessPrivacyInfoStatusEnum enumValue);

         /**
         * UPSDK 초기화 전에 호출해야 하는 게임 유저의 승인 결과를 얻습니다.
         * UPConstant.UPAccessPrivacyInfoStatusEnum를 리턴합니다.
         * 3003 및 상위 버전에서만 지원하고 있습니다.
         */
         public static UPConstant.UPAccessPrivacyInfoStatusEnum          getAccessPrivacyInfoStatus();

         /**
         * 게임 유저가 EU 지역에 있을 때.
         * UPSDK 초기화하기 전에 호출해야 하는 비동기 콜백
         * 3003 및 상위 버전에서만 지원하고 있습니다.
         */
         public static void isEuropeanUnionUser(Action <bool, string> callback);

```

### 상세 설명

#### 1. 배너 광고만 적용

Unity Plugin을 사용하여, **배너 광고만 적용하는 경우**, 광고 위치는 현재 인터페이스의 **상단 혹은 하단**에 있습니다.

#### 2. **Manifest** 패키지 이름 설정(string packagename)

이 API는 Android에서만 작동합니다. AndroidManifest에서 애플리케이션의 패키지 이름이 패키징 후 <br />
실제 이름과 다를 경우, 이 API를 사용하여 **AndroidManifest**에 해당 **패키지 이름**을 설정합니다.

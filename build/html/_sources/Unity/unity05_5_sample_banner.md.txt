## Unity Plugin 배너 광고(Banner Ad)

#### 1. 배너 광고 실행하기

```csharp
/*
* 화면 상단에 배너 광고가 실행됩니다.
* cpPlaceId는 광고 위치 식별자이니 값이 정확한지 확인합니다.
*/
public static void showBannerAdAtTop(string cpPlaceId);

/*
* 화면 하단에 배너 광고가 실행됩니다.
*/
public static void showBannerAdAtBottom(string cpPlaceId);
```


샘플：
```csharp
// 화면 상단에 'banner_aaa'의 이름으로 명명된 배너 광고가 실행됩니다.
public void onBtnBanner_Top_Click()
{
    // 아이폰X의 지원을 추가합니다. (32는 테스트용입니다.)
    UPSDK.setTopBannerForIphonex(32);
    // 화웨이 P20의 지원을 추가합니다.
    UPSDK.setTopBannerForHuaWeiP20(75);
    // 배너를 실행합니다.
    UPSDK.showBannerAdAtTop("banner_aaa");
}

// 화면 하단에 'banner_aaa'의 이름으로 명명된 배너 광고가 실행됩니다.
public void onBtnBanner_Bottom_Click()
{
    UPSDK.showBannerAdAtBottom("banner_bbb");
}
```

> 배너 광고는 상태를 확인할 필요가 없으며 적절한 시간에 호출하면 SDK가 자동으로 로드되고, <br />
로드 성공 후 자동으로 광고를 실행합니다.

#### 2. 배너 광고 숨기기
```csharp
// 상단의 배너 광고를 숨깁니다.
public static void hideBannerAdAtTop();

// 하단의 배너 광고를 숨깁니다.
public static void hideBannerAdAtBottom();
```
>  재로딩을 하지 않고도, showBannerAdAtTop() 또는 showBannerAdAtBottom()을 사용하여 배너 광고를 다시 <br />
실행 시킬 수 있습니다.


#### 3. 배너 광고 제거하기
```csharp
// 특정 광고 위치의 배너 광고를 제거합니다. 다시 디스플레이 될 경우 다시 로드 합니다.
public static void removeBannerAdAt(string cpPlaceId);
```

샘플:

```csharp
//  'banner_aaa'라는 배너 광고를 제거합니다.
public void onBtnBanner_TopRemove_Click()
{
    UPSDK.removeBannerAdAt("banner_aaa");
}

```
#### 4. 상단 배너 위치 조정하기

```csharp
// 아이폰X용
void setTopBannerForIphonex(int padding);
// 화웨이 P20용
void setTopBannerForHuaWeiP20(int padding);
```
샘플:
```csharp
// 상단 배너를 32픽셀만큼 아래로 이동시킵니다.
UPSDK.setTopBannerForIphonex (32);
```

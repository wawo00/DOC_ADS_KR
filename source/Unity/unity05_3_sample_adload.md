

## Unity Plugin 광고 로드

#### 1. 삽입 광고(Interstitial Ad) 로딩의 콜백
```csharp

/*
* 삽입 광고 로딩을 위한 API 콜백을 추가합니다.
* @param cpPlaceId:  	삽입 광고의 표시이며, 공백 혹은 null이 되면 안됩니다.
* @param success 	    로딩 성공의 콜백
* @param fail     	  로딩 실패의 콜백
*
* 콜백 매개변수의 유형: Action <string,string>
* 첫 번째는 cpPlaceID이며 placementId를 나타내며, 공백 혹은 null 입니다. 두 번째 매개변수는 정보 전달 기능이며, 공백 혹은 null 입니다.
* 2028 버전부터 지원하고 있습니다.
*/static void setIntersitialLoadCallback(string cpPlaceId, Action <string,string> success, Action <string, string> fail)
```

샘플:

```csharp

//이 메소드를 사용하여 "inter_aaa"의 콜백 성공 실패 여부를 확인합니다.
public void onBtn_ClickForIntsLoadCallback() {
    // "inter_aaa"는 placementId입니다.
    UPSDK.setIntersitialLoadCallback ("inter_aaa",
        new System.Action <string, string>(actionForIntsLoadSuccess),
        new System.Action <string, string>(actionForIntsLoadFail)
    );
}

private void actionForIntsLoadFail(string placeId, string msg)
{
    Debug.Log ("===> actionForIntsLoadFail Callback at: " + placeId);
}

private void actionForIntsLoadSuccess(string placeId, string msg)
{
    Debug.Log ("===> actionForIntsLoadSuccess Callback at: " + placeId);
}


```

#### 2. 리워드 동영상 광고 로딩의 콜백

```csharp
/*
* 리워드 동영상 광고 로딩을 위한 API 콜백을 추가합니다.
* @param success 	로딩 성공의 콜백
* @param fail     	로딩 실패의 콜백
*
* 콜백 매개변수의 유형: Action <string,string>
* 첫 번째는 cpPlaceID이며 placementid를 나타내며, 공백 혹은 null 입니다. 두 번째 매개변수는 정보 전달 기능이며, 공백 혹은 null 입니다.
* 2028 버전부터 지원하고 있습니다.
 */
public static void setRewardVideoLoadCallback(Action <string,string> success, Action <string, string> fail)


```
샘플：
```csharp

// 이 메소드에는 매개변수가 존재하지 않습니다.
public void onBtn_ClickForRewardLoadCallback() {
    Polymer.PolyADSDK.setRewardVideoLoadCallback (
        new System.Action <string, string>(actionForRewardLoadSuccess),
        new System.Action <string, string>(actionForRewardLoadFail)
    );
}

//Call this method after RewardVideo Ad loading unsuccessfully
//parameters：placeId is not required or a specific placementid，msg means the reason of failture
private void actionForRewardLoadFail(string placeId, string msg)
{
    Debug.Log ("===> actionForRewardLoadFail Callback at: " + placeId + ", fail reason: " + msg);
}

// 리워드 동영상 광고가 성공적으로 로딩되지 않았을 때, 이 메소드를 호출합니다.
// 매개변수: placeId는필수 사항이 아니거나 광고의 특정 위치이며, msg는 실패 원인을 알려줍니다.
private void actionForRewardLoadSuccess(string placeId, string msg)
{
    Debug.Log ("===> actionForRewardLoadSuccess Callback at: " + placeId);
}


```

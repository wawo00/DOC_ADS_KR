## Unity Plugin A/B 테스트

#### 1. A/B 테스트 초기화
```csharp
/*
 * A/B 테스트의 광고 구조를 받기 전에 API를 통하여 A/B 테스트가 초기화되어야 합니다.
 */
public static void initAbtConfigJson(string gameAccountId, bool completeTask, int isPaid, string promotionChannelName,  string gender, int age, string[] tags) ;
```
샘플:

```csharp
public void onBtnInitABConfig_Click()
{
    // 다음의 매개 변수는 시험용이라 아무런 의미가 없습니다.
    UPSDK.initAbtConfigJson("mygameAccountId_123", true, 18, "324000", "gender", 33, new string[]{"This is the first element.", "The second one.", "The last one."});
}
```
#### 2.  A/B 테스트 구조 받기

```csharp
  /*
	 * UPSDK 광고의 A/B 테스트 구조를 받습니다.
   * 리턴 결과는 Json 문자열일 수도 있으며, null일 수 있습니다.
   * 이 API를 호출하기 전에, initAbtConfigJson()를 먼저 호출하여 A/B Test 구조를 완성합니다.
	 */
public static string getAbtConfig(string placementId)；

```

샘플：

```csharp
public void onBtnGetABConfig_Click()
{   
	// 매개변수는 placementId 입니다.
    string r = UPSDK.getAbtConfig ("hello");
    Debug.Log ("==> onBtnGetABConfig_Click:" + r);
}
```

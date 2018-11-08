## Unity Plugin 초기화
```csharp
/*
 * UPSDK 초기화
 * UPSDK는 여러 번을 호출되더라도 한 번만 초기화됩니다.
 *  @param zone-- 0: 중국을 제외한 해외, 1: 중국,  2: IP에 의해 자동 설정
 * 3003 버전부터, UPConstant 클래스에 있는 상수를 호출합니다.
 * SDKZONE_FOREIGN, SDKZONE_CHINA, SDKZONE_AUTO;
 * 3003 하위 버전에서는 PolyADSDK 클래스를 통해 상수를 호출합니다.
 */
public static string initPolyAdSDK(int zone)
```
샘플
Demo 코드에서는 UI에서 초기화 버튼을 클릭하여 onButton_init_Click() 메소드가 호출됩니다. 초기화 버튼을 <br />
여러 번 클릭하여 UPSDK.initPolyAdSDK()를 반복해서 호출해도, SDK는 한 번만 초기화됩니다.
```csharp
public void onButton_init_Click()
{
    //TextEditor text = GameObject.Find ("CallText").GetComponent<TextEditor>();


    if (!inited) {
        UPSDK.UPSDKInitFinishedCallback = new System.Action&ltbool, string>(actionForSdkInitFinish);
        UPSDK.UPInterstitialDidClickCallback = new System.Action&ltstring, string>(actionForInterstitialDidClick);
        UPSDK.UPInterstitialDidCloseCallback = new System.Action&ltstring, string>(actionForInterstitialDidClose);
        UPSDK.UPInterstitialDidShowCallback = new System.Action&ltstring, string>(actionForInterstitialDidShow);

        UPSDK.UPBannerDidShowCallback = new System.Action&ltstring, string>(actionForSdkBannerDidShow);
        UPSDK.UPBannerDidClickCallback = new System.Action&ltstring, string>(actionForSdkBannerDidClick);
        UPSDK.UPBannerDidRemoveCallback = new System.Action&ltstring, string>(actionForSdkBannerRemove);

        UPSDK.UPRewardDidOpenCallback = new System.Action&ltstring, string>(actionForSdkRewardDidOpen);
        UPSDK.UPRewardDidClickCallback = new System.Action&ltstring, string>(actionForSdkRewardDidClick);
        UPSDK.UPRewardDidCloseCallback = new System.Action&ltstring, string>(actionForSdkRewardDidClose);
        UPSDK.UPRewardDidGivenCallback = new System.Action&ltstring, string>(actionForSdkRewardDidGiven);
        UPSDK.UPRewardDidAbandonCallback = new System.Action&ltstring, string>(actionForSdkRewardDidAbandon);

        #if UNITY_ANDROID && !UNITY_EDITOR

            UPSDK.UPExitAdDidShowCallback = new System.Action&ltstring> (actionForSdkExitAdDidShow);
            UPSDK.UPExitAdDidClickCallback = new System.Action&ltstring> (actionForSdkExitAdDidClick);
            UPSDK.UPExitAdDidClickMoreCallback = new System.Action&ltstring> (actionForSdkExitAdDidClickMore);
            UPSDK.UPExitAdOnExitCallback = new System.Action&ltstring> (actionForSdkExitAdOnExit);
            UPSDK.UPExitAdOnCancelCallback = new System.Action&ltstring> (actionForSdkExitAdOnExit);

        #endif
    }
    inited = true;

    string tt = UPSDK.initPolyAdSDK (0);
    Debug.Log ("initPolyAdSDK ====> " + tt);
}
```

콜백 인터페이스의 구현은 다음과 같습니다:  샘플 프로젝트의 코드는 각 인터페이스의 역할과 매개 변수를 나타냅니다. <br />
필요하지 않은 경우 플러스 콜백 인터페이스를 구현할 필요는 없습니다.


```csharp
#if UNITY_ANDROID && !UNITY_EDITOR
private void actionForSdkExitAdDidShow(string msg) {
    Debug.Log ("===> actionForSdkExitAdDidShow Callback");
}

private void actionForSdkExitAdDidClick(string msg) {
    Debug.Log ("===> actionForSdkExitAdDidClick Callback");
}

private void actionForSdkExitAdDidClickMore(string msg) {
    Debug.Log ("===> actionForSdkExitAdDidClickMore Callback");
}

private void actionForSdkExitAdOnExit(string msg) {
    Debug.Log ("===> actionForSdkExitAdOnExit Callback");
}

private void actionForSdkExitAdDidOnCancel(string msg) {
    Debug.Log ("===> actionForSdkExitAdDidOnCancel Callback");
}
#endif

// test for reward video callback
private void actionForSdkRewardDidOpen(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidOpen Callback at: " + placeId);
}

private void actionForSdkRewardDidClick(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidClick Callback at: " + placeId);
}

private void actionForSdkRewardDidClose(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidClose Callback at: " + placeId);
}

private void actionForSdkRewardDidGiven(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidGiven Callback at: " + placeId);
}

private void actionForSdkRewardDidAbandon(string placeId, string msg) {
    Debug.Log ("===> actionForSdkRewardDidAbandon Callback at: " + placeId);
}

private void actionForSdkBannerRemove(string placeId, string msg) {
    Debug.Log ("===> actionForSdkBannerRemove Callback at: " + placeId);
}

private void actionForSdkBannerDidClick(string placeId, string msg) {
    Debug.Log ("===> actionForSdkBannerDidClick Callback at: " + placeId);
}

private void actionForSdkBannerDidShow(string placeId, string msg) {
    Debug.Log ("===> actionForSdkBannerDidShow Callback at: " + placeId);
}

private void actionForSdkInitFinish(bool result, string msg) {
    Debug.Log ("===> actionForSdkInitFinish Callback r: " + result + ", msg: " + msg);
}

private void actionForInterstitialDidShow(string placeId, string msg) {
    Debug.Log ("===> actionForInterstitialDidShow Callback at: " + placeId);
}

private void actionForInterstitialDidClick(string placeId, string msg) {
    Debug.Log ("===> actionForInterstitialDidClick Callback at: " + placeId);
}

private void actionForInterstitialDidClose(string placeId, string msg) {
    Debug.Log ("===> actionForInterstitialDidClose Callback at: " + placeId);
}
```

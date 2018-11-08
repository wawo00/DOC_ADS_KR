## SDK 초기화

`Application` 섹션에 아래의 코드를 입력하시기 바랍니다.

```java
UPAdsSdk.init(final Context context, final UPAdsGlobalZone zone);

    public enum UPAdsGlobalZone {
        UPAdsGlobalZoneForeign,     //해외
        UPAdsGlobalZoneDomestic,    //중국
        UPAdsGlobalZoneAuto,        //ip에 의해 자동 설정
    }
```

### SetCustomeId

GAID는 중국에서 정상적으로 수집될 수 없기 때문에 중국에서 제품을 릴리즈 하시려면 <br />
`UPAdsSdk.init()`전에  이 메소드를 호출하고 customerId 매개변수의 값을 취하시기 바랍니다.
```java
public static void setCustomerId(String customerId)
```
### 디버그 Model

결합 작업 중에 디버그하면, 상세한 로그 정보를 확인할 수 있습니다. <br />
단, 제작 단계로 릴리즈하기 전에 반드시 로그 정보를 닫아야 합니다.

    UPAdsSdk.setDebuggable(true);

## 빠른 배너 광고(Banner Ad)

배너 광고를 프로젝트의 상단이나 하단에만 추가하길 원하시면, 아래의 권장 방법을 통해 <br />
더욱 빠르고 간편하게 결합할 수 있습니다.

> 매개변수 `Placement ID` 를 의미 있는 이름으로 설정할 수 있습니다. 정확한 판단이 어렵다면, <br />
UPLTV의 기술 지원팀 담당자와 상의하시기 바랍니다. UPLTV 는 각 `Placement ID`에서 발생한 수익을 <br />
제공하고 있으므로, 광고 노출 위치마다 다른 `Placement ID`를 사용해야 합니다. <br /> 
가령, 게임의 정지 화면에서 UPSDK의 초기화 설정할 때는 "정지" 또는 "메뉴"를 사용할 수 있습니다.

#### 샘플
```objective-c
/*
* 초기화 메소드
* 1. avidPlacement 매개변수, Placement ID, 유형은 NSString이며 광고 유형 표시에 사용되므로 공백으로 남겨두면 안됩니다.
* 2. vc: 유형은 UIViewController, 배너 Redirection에 사용되므로 공백으로 남겨두면 안됩니다.
* 3. showLocation: 배너의 위치 (상단 or 하단)
*/
- (instancetype)initWithPlacement:(NSString *)avidPlacement controller:(UIViewController*)vc showLocation:(UPStripShowLocationType)type;
```
스크린의 상단 혹은 하단에 광고를 설정하시려면 아래와 같은 순서를 통해 설정하실 수 있습니다.

```objective-c
typedef NS_ENUM(NSUInteger, UPStripShowLocationType) {
    UPStripShowLocationTypeTop       = 1,        //상단
    UPStripShowLocationTypeBottom    = 2,        //하단
};
```
- UPStripShowLocationTypeTop 부분을 설정하시면 스크린의 상단에 자동으로 배너가 디스플레이 됩니다.
- UPStripShowLocationTypeBottom 부분을 설정하시면, 스크린의 하단에 자동으로 배너가 디스플레이 됩니다.


샘플:

`ViewController.m` 를 통해 배너 광고 초기화와 로딩 콜백을 해드릴 수 있습니다.
```objective-c
- (void)viewDidLoad {
	// …
	//  wrapper 대상 초기화
    _topStripBanner = [[UPBannerStripWrapper alloc] initWithPlacement:@"banner_strip” controller:self showLocation:UPStripShowLocationTypeTop];
	// 프록시 콜백 설정
    _topStripBanner.delegate = self;
	// …
}
```

다른 코드를 더 입력할 필요 없이, 배너가 자동으로 ViewController 스크린 상단에 디스플레이 됩니다.

<br>

### 메모리 리사이클(Recycle memory)
배너를 볼 수 있는 ViewController가 손상(인터페이스에서 제거되거나, nav(NavigitionController)에서 팝업, <br />
혹은 메모리가 리사이클 되는 등)되더라도 리사이클 할 수 있도록 베너를 nil로 설정하시기 바랍니다.

<br>

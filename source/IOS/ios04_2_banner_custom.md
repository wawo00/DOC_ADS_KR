## 맞춤형 배너 광고(Banner Ad)

UPSDK는 `UPBannerStripWrapper`의 가로형 배너와 `UPBannerRectangleWrapper`의 사각형 배너 두 가지 스타일의 <br />
배너 광고를 제공하고 있습니다. 원하시는 광고 스타일에 따라 Wrapper 유형을 설정하시기 바랍니다.

### 배너 *.h 파일 불러오기

배너에 두 개의 헤더파일(`UPBannerStripWrapper.h`와 `UPBannerRectangleWrapper.h`)이 있습니다.

헤더 파일을 아래와 같이 설정합니다.

```objective-c
#import    <UPSDK/UPSDK.h>
```

<br>

### Wrapper 객체 설정

> 매개변수 `Placement ID` 를 의미 있는 이름으로 설정할 수 있습니다. 정확한 판단이 어렵다면, <br />
UPLTV의 기술 지원팀 담당자와 상의하시기 바랍니다. UPLTV 는 각 `Placement ID`에서 발생한 수익을 <br />
제공하고 있으므로, 광고 노출 위치마다 다른 `Placement ID`를 사용해야 합니다. <br />
가령, 게임의 정지 화면에서 UPSDK의 초기화 설정할 때는 "정지" 또는 "메뉴"를 사용할 수 있습니다.

### 설정 방법

아래와 같은 방법을 통해 배너의 디스플레이 포지션을 자유롭게 컨트롤 하실 수 있습니다.

```objective-c
/**
* 1. avidPlacement 매개변수, Placement ID, 유형은 NSString이며 광고 유형 표시에 사용되므로 공백으로 남겨두면 안됩니다.
* 2. vc: 유형은 UIViewController, 배너 Redirection에 사용되므로 공백으로 남겨두면 안됩니다.
*/
- (instancetype)initWithPlacement:(NSString *)avidPlacement controller:(UIViewController*)vc;
```

샘플:

ViewController.m,에서 사각형 배너를 초기화하고, 콜백 프록시를 설정합니다.

```objective-c
- (void)viewDidLoad {
	// …
	// wrapper 초기화
    _bottomRectBanner = [[UPBannerRectangleWrapper alloc] initWithPlacement:@"banner_rect_bottom” controller:self];
	// 콜백 설정
    _bottomRectBanner.delegate = self;
	// 배너 광고 디스플레이하는 UIView 초기화, size는 unknown
    _bannerRectView = [[UIView alloc] initWithFrame:CGRectMake(0, 0, 0, 0)];
    [self.view addSubview:_bannerRectView];
    // 광고 객체 UIView 획득 및 로드하기
    UIView *view = [_bottomRectBanner getView];
    [_bannerRectView addSubview:view];
	// …
}
```


<br>

### 배너 광고 콜백

UPBannerWrapperProtocol 는 아래 두개의 인터페이스를 포함하는 배너의 콜백입니다.

```objective-c
- (void)bannerAdClick:(id)wrapper;

- (void)bannerAdDidShow:(id)wrapper size:(CGSize)size;
```

디스크립션:

1. -(void)bannerAdClick:(id)wrapper;는 배너가 클릭되었는지 확인하는데 사용됩니다. 배너가 유저에 의해 <br />
클릭되면 wrapper로부터 인터페이스로 메시지가 전송됩니다.

2. -(void)bannerAdDidShow:(id)wrapper size:(CGSize)size; 는 배너가 디스플레이 되었을 때, <br />
wrapper로부터 인터페이스로 메시지가 전송됩니다. 파라미터(parameter)의 크기는 실제 디스플레이된  <br />
배너 사이즈입니다. 일반적으로 배너의 레이아웃은 이 인터페이스를 통해 조정됩니다.

샘플：
```objective-c
/**
 *  
 *
 *  @param wrapper 광고 대상
 */
- (void)bannerAdDidShow:(id)wrapper size:(CGSize)size{
    if (_bottomRectBanner == wrapper){
        CGRect frame = _bannerRectView.frame;
        frame.size = size;
        // bannerRecView가 상위 클래스에서 가운데 정렬 되도록합니다
        frame.origin.x = (self.view.frame.size.width - frame.size.width)/2;
        frame.origin.y = self.view.frame.size.height - frame.size.height;
        _bannerRectView.frame = frame;
        // banner RectView를 실행하기
        _bannerRectView.hidden = NO;
    }
}


/**
 *  광고 클릭
 *
 *  @param wrapper wrapper 객체
 */
- (void)bannerAdClick:(id)wrapper{
    // TODO
}
```

<br>

### 메모리 리사이클(Recycle memory)

배너를 볼 수 있는 ViewController가 손상(인터페이스에서 제거되거나, nav(NavigitionController)에서 팝업, <br />
메모리가 리사이클 되는 등)되더라도 리사이클 할 수 있도록 베너를 nil로 설정하시기 바랍니다.

<br>

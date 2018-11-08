
## 삽입 광고(Interstitial Ad)

관련 방법과 프록시 인터페이스는 `UPIntersitialWrapper.h` 파일에 정의되어 있습니다.

### 참조 해더 파일
```objective-c
#import   <UPSDK/UPSDK.h>
```
### implementation class 메소드

> 매개변수 `Placement ID` 를 의미 있는 이름으로 설정할 수 있습니다. 정확한 판단이 어렵다면, <br />
UPLTV의 기술 지원팀 담당자와 상의하시기 바랍니다. UPLTV 는 각 `Placement ID`에서 발생한 수익을 <br />
제공하고 있으므로, 광고 노출 위치마다 다른 `Placement ID`를 사용해야 합니다. <br />
가령, 게임의 정지 화면에서 UPSDK의 초기화 설정할 때는 "정지" 또는 "메뉴"를 사용할 수 있습니다.

```objective-c
@interface UpIntersitialWrapper : NSObject

/**
* 초기화 메소드
* @ param avidPlacement：Placement ID
**/
- (instancetype)initAvidPlacement:(NSString *)avidPlacement;

/**
* 콜백 객체 프록시 설정하기
* @ param delegate：UpIntersitialDelegate
**/
- (void)setDelegate:(id<UpIntersitialDelegate>)delegate;

/**
* 비디오 광고가 재생 준비 되었는지 확인하기
**/
- (BOOL)isReady;

/**
* 콜백 객체 프록시 설정하기
* @ param viewController: UIViewController 객체, 클릭시 스킵 위치 설정
**/
- (BOOL)show:(UIViewController *)viewController;

/**
 * 광고 로딩 콜백
 * @param delegate，콜백 프록시
 **/
 - (void)load:(id<UPIntersitialLoadDelegate>)delegate;
@end
```
### UpIntersitialDelegate 콜백 프로토콜
삽입 광고의 콜백 프로토콜을 설정할 수 있습니다. <br />
이는 선택사항이며, 필요하시면 아래의 방법을 통해 구현 하실 수 있습니다.

```objective-c
@protocol UpIntersitialDelegate <NSObject>

/**
 * 삽입 광고가 디스플레이 되면 호출됩니다
 * @ param wrapper ：sender, UpIntersitialWrapper object
 */
- (void)interstitialAdDidShow:(UpIntersitialWrapper *)wrapper;

/**
 * 삽입 광고가 닫혔을 때, 호출됩니다.
 * @ param wrapper sender, UpIntersitialWrapper object
 */
- (void)interstitialAdDidClose:(UpIntersitialWrapper *)wrapper;

/**
 * 삽입 광고가 클릭됬을 때 호출됩니다.
 * @ param wrapper sender, UpIntersitialWrapper object
 */
- (void)interstitialAdDidClick:(UpIntersitialWrapper *)wrapper;

@end

```

### Demo 코드 - Xcode

이 부분에서는 Xcode를 사용한 예시만 설명드리고 있습니다. <br />
다른 개발 툴을 사용하고 계신 분들께 불편을 끼쳐 드려 죄송합니다.

Xcode의 Demo 프로젝트에서,
`STInterstitialViewController` 클래스를 선언하여 삽입 광고를 디스플레이 합니다. <br />
먼저, `STInterstitialViewController.h`를 아래와 같이 정의합니다.

```objective-c
@interface STInterstitialViewController : UIViewController

@end
```

삽입 광고의 로딩 및 광고 실행을 완성하기 위해 `STInterstitialViewController.m`에서 몇 줄의 코드 라인을 작성합니다. <br />
먼저, `UpIntersitialWrapper:`객체 `_intersitialWrapper`를 정의합니다. 이 객체를 가지고 삽입 광고의 로딩 및 실행을 <br />
컨트롤 할 수 있습니다. 삽입 광고 실행 위치마다 다른 `UpIntersitialWrapper` 의 객체를 사용해야 합니다.

코드:

```objective-c
@interface STInterstitialViewController () <UpIntersitialDelegate>
{
// wrapper 객체를 선언합니다. 필요시, 더 많은 객체 선언을 할 수도 있습니다.
    UpIntersitialWrapper *_intersitialWrapper;
}
@end
```

아래 코드를 추가합니다.

```objective-c

- (void)viewDidLoad  {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor whiteColor];

    // viewDidLoad에 버튼을 추가하여 삽입 광고 로딩 및 승인을 테스트합니다.
    UIButton *button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
    button.backgroundColor = [UIColor orangeColor];
    button.frame = CGRectMake(self.view.frame.size.width/2 - 250/2, 100, 250, 40);
    [button setTitle:@"Interstital Ad Test" forState:UIControlStateNormal];
    // 버튼 클릭시 intersitialClick 가 호출됩니다.
    [button addTarget:self action:@selector(intersitialClick) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:button];

}

- (void)intersitialClick {
    // inter_bbb가 _intersitialWrapper에 해당되는 광고 위치라고 가정합니다.
    _intersitialWrapper = [[UpIntersitialWrapper alloc] initAvidPlacement:@"inter_bbb"];
    // 콜백 프록시를 설정합니다.
    [_intersitialWrapper setDelegate:self];

    // 삽입 광고가 준비되었는지 확인합니다.
    if ([_intersitialWrapper isReady]) {
        [_intersitialWrapper show:self];
    }
    else
    {
        NSLog(@"Intersitial no ready");
    }
}
```

아래의 3단계 코딩을 통해 삽입 광고 디스플레이 및 컨트롤이 가능합니다.

1. 글로벌 객체를 선언합니다.
    ```objective-c
     UpIntersitialWrapper *_intersitialWrapper;
    ```
2. 객체를 아래와 같이 설정합니다.
    ‘inter_bbb’는 실제 광고 위치입니다. 실제 광고 위치와 다를 경우 <br />
    광고 로딩에 문제를 야기할 수 있습니다.
    ```objective-c
    _intersitialWrapper = [[UpIntersitialWrapper alloc] initAvidPlacement:@"inter_bbb"];
    ```
3. 아래와 같은 코드를 사용하여 광고를 디스플레이합니다.
```objective-c
if ([_intersitialWrapper isReady]) {
     [_intersitialWrapper show:self];
}
```

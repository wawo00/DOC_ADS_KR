## 리워드 동영상 광고(Rewarded Video Ad)  

리워드 동영상 인터페이스 및 프로토콜은 `UPRewardWrapper.h` 파일에 정의되어 있습니다.

헤더파일 `UPRewardWrapper.h`를 Xcode 프로젝트에 추가합니다.
```objective-c
#import    <UPSDK/UPSDK.h>
```

리워드 동영상 광고는 싱글턴 디자인 모델을 사용한다는 점에서 삽입 광고와 다르지만, <br />
인터페이스와 메소드는 삽입 광고와 비슷해 보입니다.

`UPRewardWrapper.h` 메소드：

    @interface UPRewardWrapper : NSObject

    /*
     * wrapper 의 단독 객체 가져오기
     */
    + (instancetype)shareInstance;

    /*
     * 콜백 프록시 설정하기 (선택 사항입니다)
     */
    - (void)setDelegate:(id<UPRewardDelegate>)delegate;

    /*
     * 동영상이 준비되었는지 확인합니다
     */
    - (BOOL)isReady;

    /**
     * 동영상 실행하기
     * @param viewController, redirection을 위해 사용 되므로 정확하게 입력하시기 바랍니다.
     * @param adid, Ad Placement ID, 트래킹과 분석용으로 맞춤 설정합니다.
     **/

    - (BOOL)show:(UIViewController *)viewController placeId:(NSString*)adId;
    /**
    * 광고 로딩의 콜백
    * @param delegate, 콜백 프록시
    **/
    - (void)load:(id<UPRewardLoadDelegate>)delegate;
    @end

리워드 동영상 광고는 삽입 광고 및 배너 광고와 마찬가지로 콜백을 프록시 프로토콜로 정의합니다.  <br />
`UPRewardDelegate`를 `UPRewardWrapper.h`에서 선언합니다.

```objective-c
@protocol UPRewardDelegate <NSObject>

/*
 * 리워드 동영상 열기
 */
- (void)UPRewardVideoAdDidOpen;

/*
 * 리워드 동영상 클릭하기
 */
- (void)UPRewardVideoAdDidCilck;

/*
 * 리워드 동영상 닫기
 */
- (void)UPRewardVideoAdDidClose;

/*
 * 보상 준비하기
 * @param reward: 보상 관련 데이터입니다.
 */
- (void)UPRewardVideoAdDidRewardUserWithReward:(NSDictionary *)reward;

/*
 * 조건을 만족하지 못했을 때, 보상을 주지 않습니다.
 * @param error: 조건을 만족하지 못한 사유
 */
- (void)UPRewardVideoAdDontReward:(NSError *)error;

@end
```

### 코드 예시

`STRewardViewController`클래스를 정의하여 리워드 동영상을 테스트 합니다.
> 이 부분에서는 Xcode를 사용한 예시만 설명드리고 있습니다. <br />
다른 개발 툴을 사용하고 계신 분들께 불편을 끼쳐 드려 죄송합니다.

    @interface STRewardViewController : UIViewController

    @end

>  `STRewardViewController`에 버튼을 추가하여 간편하게 리워드 동영상을 확인할 수 있습니다.

아래의 코드를 `STRewardViewController.m` 파일에 추가합니다.

```objective-c

#import    <UPSDK/UPSDK.h>

@interface STRewardViewController () <UPRewardDelegate>

@end
```

`STRewardViewController`는 프로토콜 `UPRewardDelegate`를 실행하여 동영상 실행에 관련된 사항을 모니터링 할 수 있습니다.

아래의 코드를 이어서 추가합니다.

```objective-c
- (void)viewDidLoad {
    [super viewDidLoad];
    self.view.backgroundColor = [UIColor whiteColor];

    UIButton *button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
    button.backgroundColor = [UIColor orangeColor];
    button.frame = CGRectMake(self.view.frame.size.width/2 - 250/2, 100, 250, 40);
    [button setTitle:@"Reward Video" forState:UIControlStateNormal];
    [button addTarget:self action:@selector(rewardClick) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:button];

    [[UPRewardWrapper shareInstance] setDelegate:self];
}

- (void)rewardClick
{
    if ([[UPRewardWrapper shareInstance] isReady]) {
        [[UPRewardWrapper shareInstance] show:self placeId:@"aaaa"];
    }
    else
    {
        NSLog(@"Reward no ready");
    }
}

```

리워드 동영상 광고의 메인 테스팅 코드가 완성되었습니다. 이 프로젝트를 실행 한 후 `Rewarded Video` 버튼을 클릭하여 <br />
동영상 광고를 볼 수 있습니다. `Reward not ready` 문구가 나오면, 네트워크 상태를 확인해 주시기 바랍니다.

마지막으로 동영상 프록시 프로토콜을 위한 코드를 추가합니다.

```objective-c
#pragma mark - UPRewardDelegate
//리워드 동영상 광고 실행하기
- (void)UPRewardVideoAdDidOpen
{
    NSLog(@"UPRewardVideoAdDidOpen");
}

//리워드 동영상 광고 클릭하기
- (void)UPRewardVideoAdDidCilck
{
    NSLog(@"UPRewardVideoAdDidCilck");
}

//리워드 동영상 광고 닫기
- (void)UPRewardVideoAdDidClose
{
    NSLog(@"UPRewardVideoAdDidClose");
}

//리워드 동영상을 통해 보상하기. 만약 이 메소드를 호출한다면 게임 유저에게 보상이 지급됩니다.
- (void)UPRewardVideoAdDidRewardUserWithReward:(NSDictionary *)reward
{
    NSLog(@"UPRewardVideoAdDidRewardUserWithReward");
}

//게임 유저에게 보상하지 않기. 만약 이 메소드를 호출한다면 조건을 만족시키지 못했으므로 게임 유저에게 보상이 지급되지 않습니다.
- (void)UPRewardVideoAdDontReward:(NSError *)error
{
    NSLog(@"UPRewardVideoAdDontReward");
}
```
> 동영상이 디스플레이 된 후의 기록들을 확인하실 수 있습니다. 이를 통해 동영상이 정상적으로 실행 되었는지 <br />
확인 하실 수 있습니다.

<font color=#DC143C>참고: `UPRewardVideoAdDidRewardUserWithReward`를 받은 후에 게임 유저(player)에게 보상(reward)을 줄 수 있습니다.</font>

### 로딩 콜백

`[[UPRewardWrapper shareInstance] isReady]`를 이용하여 광고가 정상적으로 재생 되었는지 확인 할 수 있습니다.


광고 로드 콜백이 필요하신 분은 아래를 참고하시기 바랍니다.

```
// 해당 클래스에서 UPRewardLoadDelegate 프로토콜을 선언하고 메소드를 호출합니다.
[[UPRewardWrapper shareInstance] load:self];
```

```
// 성공된 콜백을 로딩합니다.
- (void)UPRewardVideoAdDidLoad {
    NSLog(@"reward video ad loaded successfully");
}
// 실패한 콜백을 로딩합니다.
- (void)UPRewardVideoAdDidLoadFail {
    NSLog(@"reward video ad failed to load");
}
```

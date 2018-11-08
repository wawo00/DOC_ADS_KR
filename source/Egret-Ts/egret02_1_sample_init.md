##  SDK 초기화

이 부분에서는 Xcode를 사용한 샘플만 설명드리고 있습니다. <br />
다른 개발 툴을 사용하고 계신 분들께 불편을 끼쳐 드려 죄송합니다.

```typescript
/*
* 다른 API 인터페이스의 SDK를 사용하기 전에 SDK 초기화를 완료합니다.
* @param zone product distribution area, 0: 중국을 제외한 해외, 1: 중국, 2: IP에 의해 자동 설정
* @param callback SDK 초기화가 완성된 후 콜백되어 성공적으로 완료됨을 알려줍니다.
*/
static initSDK(zone: number, callback?:(res:boolean)=>void)
```
샘플:
```typescript
let initButton = new eui.Button();
initButton.label = "initButton";
this.addChild(initButton);
initButton.addEventListener(egret.TouchEvent.TOUCH_TAP, this.initButtonClick, this);

initButtonClick(e: egret.TouchEvent){
        upltv.initSDK(0);
}
```

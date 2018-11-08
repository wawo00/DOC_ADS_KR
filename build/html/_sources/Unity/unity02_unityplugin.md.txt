## Unity Plugin 추가하기

다운로드 된 UPSDK Unity Plugin은 *.unitypackage 파일입니다. 이 문서에서는 두 가지 플랫폼의 대표적인 예시를 <br />
설명하고 있으니, 아래를 참고하시기 바랍니다.

> 이 절차를 숙지하고 계신 개발자 분께서는, 다음 섹션으로 넘어갑니다.

### 1. Plugin 업데이트하기

이전에 UPSDK Unity Plugin을 결합한 경우, 업데이트 후 Plugin 라이브러리가 올바른지 확인하기위해 업데이트 전에, UPSDK Unity Plugin을 제거합니다.

Unity 프로젝트에 `Assets/PolyADSDK/Plugins/` 과 `Assets/StreamingAssets/UPSDK_android/`가 존재하는지 확인합니다. <br />
일반적으로 `PolyADSDK` 및 `UPSDK_android` 폴더(하위 카탈로그 포함)를 삭제하면 전체 불러온 Plugin이 완전히 삭제됩니다.

`PolyADSDK` 카탈로그의 이름 또는 하위 카탈로그의 위치를 편집한 경우 수정된 위치를 찾아 삭제합니다. <br />
특히 `PolyADSDK`를 업데이트한 후 일부 중요 파일을 추가한 경우 삭제 시 유의합니다.

### 2. 패키지 불러오기

프로젝트에서 Asset을 선택하고 마우스 오른쪽 버튼을 클릭한 후 `Import Package` -> `Custom Package`를 선택합니다.

![import](http://docc.upltv.com/uploads/201705/592e66a727d89_592e66a7.png "import")

### 3. Plugin 패키지 선택하기

 다운로드한 Plugin 패키지(예: export.unitypackage)를 선택합니다. 그런 다음 `open`버튼을 클릭합니다. <br />
 Unity가 Plugin 패키지를 자동으로 로드합니다.

![open file](http://docc.upltv.com/uploads/201705/592e684f54573_592e684f.png "open file")

### 4. 패키지 로드하기

Unity가 Plugin 패키지 로드를 마치면 아래와 같은 Plugin 리소스를 불러올 수 있는 팝업 창이 나타납니다.

![import plugin](http://docc.upltv.com/uploads/201705/592e69ba77ebf_592e69ba.png "import plugin")

일반적으로 "All" 버튼을 선택한 다음 `Import`를 클릭하여 불러옵니다. Android 플랫폼에만 지원하는 경우, <br />
**iOS**버튼은 무시합니다. iOS만 지원하는 경우에는 **Android** 버튼은 무시합니다.

### 5. 로드 완료
UPSDK의 Plugin을 불러온 후 프로젝트에 **PolyADSDK**라는 폴더가 표시됩니다. 예시로, Android플랫폼 및 iOS플랫폼 <br />
모두에 대한 지원을 하신다면 Plugin에 대한 모든 리소스를 불러오기 합니다.

![load ok](http://docc.upltv.com/uploads/201705/592e6b65dd56e_592e6b65.png "load ok")

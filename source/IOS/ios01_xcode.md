## Xcode Access Document
### Download SDK
Download latest SDK from [here](http://ads-sdk-doc.haloapps.com/docs/show/13  "SDK Download Page") and unzip. You will got 2 files：
- UPSDK.framework
- UPSDK.bundle

`UPSDK.framework` is static library of SDK
`UPSDK.bundle` contains external resources refered by static library, like images.
</br>

### Add SDK to your Xcode project
Add `UPSDK.framework` and `UPSDK.bundle` to you Xcode project , as sample `FrameWork`:
![IOS_01](http://docs.upltv.com/uploads/201808/5b88d60fe148e_5b88d60f.png "IOS_01")

### Add 3rd part library required
We need 3rd part library to display ads. Please import these libraries to your project.
**Please downlaod 3rd library from [here](http://ads-sdk-doc.haloapps.com/docs/show/13 "3rd libraries") **。

![ios_02](http://docs.upltv.com/uploads/201808/5b88d65b062ae_5b88d65b.png "ios_02")
<br>
- **3rd party library is optional, please add it according to our suggestion. If you have any problems, please contact us.**
</br>
### Add system reference library 
Add depends library in: TARGETS → General → Linked Frameworks Libraries
- `QuartzCore.framework`
- `MediaPlayer.framework`
- `libsqlite3.tbd`
- `libz.tbd`
- `CoreMedia.framework`
- `CoreGraphics.framework`
- `CFNetwork.framework`
- `WebKit.framework (Optional)`
- `WatchConnectivity.framework (Optional)`
- `SystemConfiguration.framework`
- `StoreKit.framework`
- `Social.framework`
- `MessageUI.framework`
- `JavaScriptCore.framework (Optional)`
- `EventKit.framework`
- `CoreTelephony.framework`
- `AVFoundation.framework`
- `AudioToolbox.framework`
- `AdSupport.framework`
- `GLKit.framework`
- `CoreMotion.framework`
- `libxml2.tbd`
- `libc++.tbd`
- `SafariServices.framework`
- `CoreLocation.framework`
- `EventKitUI.framework`
- `MobileCoreServices.framework`

**Attention:If you use cocos to build project,please add following library:**

- `GameController.framework`

<br>

### Project configuration 
#### 1 Add linker flags

- Add `-ObjC` and `-fobjc-arc` through `TARGETS` → `Build Setting` → `Linking` → `Other Linker Flags`  as following image:
![ios_03](http://docs.upltv.com/uploads/201808/5b88d8fb6b6a9_5b88d8fb.png "ios_03")


#### 2 Add nodes in info.plist to allow http protocol 

```objective-c
&lt;key>NSAppTransportSecurity&lt;/key>
&lt;dict>
	&lt;key>NSAllowsArbitraryLoads&lt;/key>
	&lt;true/>
&lt;/dict>
```

#### 3 Add nodes info.plist, to require permissions
```objective-c
&lt;key>NSCalendarsUsageDescription&lt;/key>
&lt;string>Some ad content may create a calendar event.&lt;/string>
&lt;key>NSCameraUsageDescription&lt;/key>
&lt;string>Some ad content may access camera to take picture.&lt;/string>
&lt;key>NSPhotoLibraryUsageDescription&lt;/key>
&lt;string>Some ad content may require access to the photo library.&lt;/string>
```

Note: you can change description into your target language if needed.
<br>

#### 4 Bitcode
We support Bitcode, please choose use Bitcode if needed.
Note: Youlan and Domob SDK do not support Bitcode.
<br>

5.Configuration of Cocos engine
If you build project with cocos,please make sure  `Version` filed has been filled,as following:

![cocos项目Version字段配置](http://docs.upltv.com/uploads/201709/59afb01ec7612_59afb01e.png "cocos项目Version字段配置")
<br>

### Version
You can find SDK version in file *UPSDKVersion.h*

```objective-c
//sdk version
#define UPSDKVERSION  @"3003"
```
> as we see,the version of upsdk is 3003 
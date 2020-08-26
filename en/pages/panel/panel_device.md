# Device Control BizBundle

## Features Overview
Tuya Smart Android Device Control BizBundle is the core container of Tuya Smart Device Control Panel, based on the Tuya Smart Android Home SDK, it provides the interface package for loading and controlling the device control panel to speed up the application development process. It mainly includes the following functions:
* Load Device Panel (Supported hardware device type: WIFI、Zigbee、Mesh、BLE)
* Device Panel Control (Supported Device and Group Control, Unsupported Group Manager)
* Device Alarm

## Integrated Device Control Biz Bundle

### Create Project

   Build your project in the Android Studio and integrate Tuyasmart HomeSDK

### Configure the root build.gradle

  ``` groovy
  allprojects {
      repositories {
          maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
          maven { url 'https://jitpack.io' }
      }
  }
  ```

### Configure the module build.gradle

  ``` groovy
  android {
        defaultConfig {
          ndk {
              abiFilters "armeabi-v7a", "arm64-v8a"
          }
      }

      repositories {
          flatDir {
              dirs 'libs'
          }
      }
      compileOptions {
          sourceCompatibility 1.8
          targetCompatibility 1.8
      }

      packagingOptions {
          pickFirst 'lib/*/libc++_shared.so'
          pickFirst 'lib/*/libgnustl_shared.so'
      }
  
    lintOptions {
          abortOnError false
      }
  }
	dependencies {
           implementation fileTree(dir: 'libs', include: ['*.jar'])
      //panel
      implementation 'com.tuya.smart:panel-sdk:0.6.0'
      //homesdk
      implementation 'com.alibaba:fastjson:1.1.67.android'
      implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
      implementation 'com.tuya.smart:tuyasmart:3.17.0'
      //third
      implementation 'com.weigan:loopView:0.1.1'
      implementation 'com.facebook.infer.annotation:infer-annotation:0.11.2'
      implementation 'javax.inject:javax.inject:1'
      implementation 'com.facebook.soloader:soloader:0.8.2'
      implementation 'com.facebook.fresco:fresco:1.3.0'
      implementation "com.facebook.fresco:imagepipeline-okhttp3:1.3.0"
      implementation 'com.alibaba:fastjson:1.1.67.android'
      implementation 'com.facebook.react:react-native:0.51.1.11'
      implementation 'org.apache.commons:commons-compress:1.9'
      implementation 'com.github.PhilJay:MPAndroidChart:v3.0.3'
      implementation 'io.reactivex.rxjava2:rxandroid:2.0.1'
      implementation 'io.reactivex.rxjava2:rxjava:2.1.7'
      implementation 'com.readystatesoftware.systembartint:systembartint:1.0.3'
      implementation 'com.android.support.constraint:constraint-layout:1.1.3'
  	}
  ```

  ### Optional Configuration

  ##### Amap  ( China only )

  ``` groovy
  implementation 'com.amap.api:search:6.9.2'
  implementation 'com.amap.api:map2d:5.2.0'
  implementation 'com.tuya.smart:tuyasmart-react-native-amap:1.0.0'
  ```

  ##### GoogleMap ( Non-China only )

  ``` groovy
  implementation 'com.tuya.smart:tuyasmart-react-native-googlemap:1.0.0'
  implementation('com.google.android.gms:play-services-maps:16.1.0') {
      exclude group: 'com.android.support'
  }  
  ```

  ##### QQ Music

  ``` groovy
  implementation(name: 'qqmusic-innovation-aidl-api-sdk-1.00-SNAPSHOT-release', ext: 'aar')
  implementation 'com.tuya.smart.rnplugin:tyrctspeakermanager:1.0.15-open'
  def tvsVer = '2.3.0'
  implementation "com.tencent.yunxiaowei.dmsdk:core:$tvsVer"
  implementation "com.tencent.yunxiaowei.webviewsdk:webviewsdk:$tvsVer"
  ```

  ##### Sweeper

  ``` groovy
  implementation 'com.tuya.smart:tuyasmart-TuyaRNApi-sweeper:5.26.13-open'
  ```

### Configure Theme 
  ``` xml
      <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
        <!-- default configuration start -->
        <item name="app_bg_color">@color/app_bg_color</item>
        <item name="list_line_color">@color/list_line_color</item>
        <!-- default configuration end -->
    </style>
  ```

### Configure the progurad

  ``` 
  # react-native
  -keep,allowobfuscation @interface com.facebook.common.internal.DoNotStrip
  -keep,allowobfuscation @interface com.facebook.proguard.annotations.DoNotStrip
  -keep,allowobfuscation @interface com.facebook.proguard.annotations.KeepGettersAndSetters
  # Do not strip any method/class that is annotated with @DoNotStrip
  -keep @com.facebook.proguard.annotations.DoNotStrip class *
  -keep @com.facebook.common.internal.DoNotStrip class *
  -keepclassmembers class * {
      @com.facebook.proguard.annotations.DoNotStrip *;
      @com.facebook.common.internal.DoNotStrip *;
  }
  -keepclassmembers @com.facebook.proguard.annotations.KeepGettersAndSetters class * {
    void set*(***);
    *** get*();
  }
  -keep class * extends com.facebook.react.bridge.JavaScriptModule { *; }
  -keep class * extends com.facebook.react.bridge.NativeModule { *; }
  -keepclassmembers,includedescriptorclasses class * { native <methods>; }
  -keepclassmembers class *  { @com.facebook.react.uimanager.UIProp <fields>; }
  -keepclassmembers class *  { @com.facebook.react.uimanager.annotations.ReactProp <methods>; }
  -keepclassmembers class *  { @com.facebook.react.uimanager.annotations.ReactPropGroup <methods>; }
  -dontwarn com.facebook.react.**
  -keep,includedescriptorclasses class com.facebook.react.bridge.** { *; }

  #Amap
  -dontwarn com.amap.**
  -keep class com.amap.api.maps.** { *; }
  -keep class com.autonavi.** { *; }
  -keep class com.amap.api.trace.** { *; }
  -keep class com.amap.api.navi.** { *; }
  -keep class com.autonavi.** { *; }
  -keep class com.amap.api.location.** { *; }
  -keep class com.amap.api.fence.** { *; }
  -keep class com.autonavi.aps.amapapi.model.** { *; }
  -keep class com.amap.api.maps.model.** { *; }
  -keep class com.amap.api.services.** { *; }

  #Google Play Services
  -keep class com.google.android.gms.common.** {*;}
  -keep class com.google.android.gms.ads.identifier.** {*;}
  -keepattributes Signature,*Annotation*,EnclosingMethod
  -dontwarn com.google.android.gms.**

  #MPAndroidChart
  -keep class com.github.mikephil.charting.** { *; }
  -dontwarn com.github.mikephil.charting.**

  -keep class com.tuya.**.**{*;}
  -dontwarn com.tuya.**.**
  ```

### Init Tuya Smart Android Device Control Biz Bundle in the Application

  ``` java
  public class TuyaSmartApp extends Application {
      @Override
      public void onCreate() {
          super.onCreate();

          TuyaHomeSdk.setDebugMode(BuildConfig.DEBUG);
          // you must remove TuyaHomeSdk.init(),then add TuyaPanelSDK.init()
          TuyaPanelSDK.init(this," TUYA_SMART_APPKEY","TUYA_SMART_SECRET");

          // fail router listener
          TuyaWrapper.init(this, new RouteEventListener() {
            @Override
            public void onFaild(int errorCode, UrlBuilder urlBuilder) {
                ToastUtil.shortToast(TuyaPanelSDK.getCurrentActivity(), urlBuilder.originUrl);
            }
          });
          //register FamilyService,set the current family homeId,BizBundleFamilyServiceImpl is sample code
          TuyaWrapper.registerService(AbsBizBundleFamilyService.class, new BizBundleFamilyServiceImpl());
      }
  }
  ```

## Function Call

### Implementation FamilyService

you must extends AbsBizBundleFamilyService class，set the current family homeId

**sample code**
``` java
public class BizBundleFamilyServiceImpl extends AbsBizBundleFamilyService {

    private long mHomeId;

    @Override
    public long getCurrentHomeId() {
        return mHomeId;
    }

    @Override
    public void setCurrentHomeId(long homeId) {
        mHomeId = homeId;
    }
}
```
### Set Family HomeId

After fetching the family list, set the current family homeId with a service call

**示例代码**
``` java
    AbsBizBundleFamilyService service = MicroServiceManager.getInstance().findServiceByInterface(AbsBizBundleFamilyService.class.getName());
    //set the current family homeId
    service.setCurrentHomeId(homeBean.getHomeId());
```

### Open panel

Enter the panel page through the homeId and deviceId.The homeId and deviceId need to be obtained through the public Tuya Smart SDK interface.

**Declaration**

Open Panel

``` java
gotoPanelViewControllerWithDevice(Activity activity, long homeId, String deviceId, ITuyaPanelLoadCallback loadCallback);
```
**Parameter**

| Parameter         | Description                                                         |
| ------------ | ------------------------------------------------------------ |
| activity     | Open the panel context must to be used TuyaPanelSDK.getCurrentActivity() |
| homeId       | The homeId need to be obtained through the public Tuya Smart Android SDK interface                              |
| deviceId        | The deviceId need to be obtained through the public Tuya Smart Android SDK interface.                                |
| loadCallback | Status Callback when the panel loads                                                |

**Example**
``` java
final ITuyaPanelLoadCallback mLoadCallback = new ITuyaPanelLoadCallback() {
    @Override
    public void onStart(String deviceId) {
        ProgressUtil.showLoading(TuyaPanelSDK.getCurrentActivity(), "Loading...");
    }
    
    @Override
    public void onError(String deviceId, int code, String error) {
        ProgressUtil.hideLoading();
        Toast.makeText(getApplicationContext(), "errorCode:" + code + ",errorString:" + error, Toast.LENGTH_LONG).show();
    }
    
    @Override
    public void onSuccess(String deviceId) {
        ProgressUtil.hideLoading();
    }
    
    @Override
    public void onProgress(String deviceId, int progress) {
    }
};
    
TuyaPanelSDK.getPanelInstance().gotoPanelViewControllerWithDevice(TuyaPanelSDK.getCurrentActivity(), mCurrentHomeId, bean.getDevId(), mLoadCallback);
```

### Panel Delegate

#### Click panle toolbar right menu

Open other pages through the callback on the toolbar right menu of the panel

**Declaration**

Click the toolbar right menu get the current panel deviceId

``` java
void setPressedRightMenuListener(ITuyaPressedRightMenuListener listener);
```
**Parameter**

| Parameter                          | Description                            |
| ----------------------------- | ------------------------------- |
| ITuyaPressedRightMenuListener | the deviceId，groupId  |
**Example**
``` java
TuyaPanelSDK.getPanelInstance().setPressedRightMenuListener(new ITuyaPressedRightMenuListener() {
    @Override
    public void onPressedRightMenu(String deviceId,long groupId) {
        Toast.makeText(TuyaPanelSDK.getCurrentActivity(), "PanelMore", Toast.LENGTH_SHORT).show();
    }
});
```

#### Get unrealized router address
Get unrealized routing address to jump to the corresponding page

**Declaration**

Go to the panel page and click the button to jump to the route, which is called if there is an unimplemented route.

``` java
void init(Application app,ITuyaOpenUrlListener listener);
```
**Parameter**

| Parameter                 | Description                     |
| -------------------- | ------------------------ |
| RouteEventListener | Unimplemented route callbacks |

**Example**
``` java
    TuyaWrapper.init(this, new RouteEventListener() {
        @Override
        public void onFaild(int errorCode, UrlBuilder urlBuilder) {
            // routing primitive address urlBuilder.originUrl
            ToastUtil.shortToast(TuyaPanelSDK.getCurrentActivity(), urlBuilder.originUrl);
        }
    });
```

### Release panle resource

Call this method to release resources when exiting the application

**Example**

``` java
TuyaPanel.getInstance().onDestroy();
```

### Clear panel cache

The panel resources files will be stored in the current app storage directory. you can call this method to clean up.

**Example**

``` java
TuyaPanelSDK.getPanelInstance().clearPanelCache();
```

## ErrorCode

| Error Code | Description                   |
| ------ | ---------------------- |
| 1901   | Panel resources download failed       |
| 1902   | multi language package download failed       |
| 1903   | Unsupported device type       |
| 1904   | No device in the group           |
| 1905   | home id is fiale            |
| 1906   | DeviceBean is null   |
| 1907   | No available panel resources found |
| 1908   | Incorrect firmware version         |
| 1909   | Unknow Error               |
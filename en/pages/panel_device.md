# Device Control BizBundle

## Features Overview
Tuya Smart Android Device Control BizBundle is the core container of Tuya Smart Device Control Panel, based on the Tuya Smart Android Home SDK, it provides the interface package for loading and controlling the device control panel to speed up the application development process. It mainly includes the following functions:
* Load Device Panel (Supported hardware device type: WIFI、Zigbee、Mesh、BLE)
* Device Panel Control (Supported Device and Group Control, Unsupported Group Manager)
* Device Alarm

## Integrated Device Control Biz Bundle

### Create Project

   Build your project in the Android Studio and integrate Tuyasmart HomeSDK and completion of the bizbundle[access](./access.md)

### Configure the module build.gradle

  ``` groovy
	dependencies {
       api 'com.tuya.smart:tuyasmart-bizbundle-panel:3.20.0-5'
  	}
  ```

  ### Optional Configuration

  ##### Amap  ( China only )

  ``` groovy
  implementation 'com.amap.api:search:6.9.2'
  implementation 'com.amap.api:map2d:5.2.0'
  ```

  ##### GoogleMap ( Non-China only )

  ``` groovy
    implementation 'com.google.android.gms:play-services-maps:17.0.0' 
  ```

  ##### QQ Music

  ``` groovy
    implementation project(':qqmusic')
    implementation("com.tencent.yunxiaowei.dmsdk:core:2.3.0") {
          exclude group: 'com.squareup.okhttp3', module: 'okhttp'
      }
    implementation("com.tencent.yunxiaowei.webviewsdk:webviewsdk:2.3.0") {
          exclude group: 'com.squareup.okhttp3', module: 'okhttp'
      }
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

## Function Call

### Open panel

Enter the panel page through the homeId and deviceId.The homeId and deviceId need to be obtained through the public Tuya Smart SDK interface.

**Declaration**

Open Panel

``` java
goPanelWithCheckAndTip(Activity activity, String devId)
```
**Parameter**

| Parameter         | Description                                                         |
| ------------ | ------------------------------------------------------------ |
| activity     | current context |                            
| devId        | The deviceId need to be obtained through the public Tuya Smart Android SDK interface.                                |

**Example**
``` java
 AbsPanelCallerService service = MicroContext.getServiceManager().findServiceByInterface(AbsPanelCallerService.class.getName());
                service.goPanelWithCheckAndTip(PanelActivity.this, bean.devId);
```

### Jump unimplemented routing address

Please check if the panel buttons do not respond when clicked. logcat content,[Jumping to an implementation page via a route callback](./access.md#application-init)


### Clear panel cache

The panel resources files will be stored in the current app storage directory. you can call this method to clean up.

**Example**

``` java
  ClearCacheService service = MicroContext.getServiceManager().findServiceByInterface(ClearCacheService.class.getName());
      if (service != null) {
        service.clearCache(PanelActivity.this);
      }
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
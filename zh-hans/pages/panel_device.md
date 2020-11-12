# 设备控制业务包

 ## 功能介绍

设备控制业务包是涂鸦智能设备控制面板的核心容器，在涂鸦智能 Android Home SDK 的基础上，提供了设备控制面板的加载和控制的接口封装，加速应用开发过程。主要包括以下功能：
* 面板加载（加载多种设备类型，支持：WIFI、Zigbee、Mesh、BLE）
* 设备控制（支持单设备和群组的控制，不支持群组管理）
* 设备定时

## 业务包集成
### 创建工程

在 Android Studio 中建立你的工程,接入公版 SDK 并完成业务包[框架接入](./access.md)

### module 的 build.gradle 配置

  ``` groovy
      dependencies {
        api 'com.tuya.smart:tuyasmart-bizbundle-panel:3.20.0-5'
      }
  ```

  ####  可选配置 根据面板功能依赖

  ##### 高德地图依赖项 发布GooglePlay时请务必去除此依赖

  ``` groovy
      implementation 'com.amap.api:search:6.9.2'
      implementation 'com.amap.api:map2d:5.2.0'
  ```

  ##### GoogleMap 依赖项 发布国内应用市场时请务必去除此依赖

  ``` groovy
      api 'com.google.android.gms:play-services-maps:17.0.0'
  ```

  ##### QQ 音乐登录模块依赖项

  ``` groovy
      api project(':qqmusic')
      api("com.tencent.yunxiaowei.dmsdk:core:2.3.0") {
          exclude group: 'com.squareup.okhttp3', module: 'okhttp'
      }
      api("com.tencent.yunxiaowei.webviewsdk:webviewsdk:2.3.0") {
          exclude group: 'com.squareup.okhttp3', module: 'okhttp'
      }
  ```
  
### 混淆配置

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

  #高德地图
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

## 功能调用

### 打开面板

通过家庭 Id 和设备 Id 进入对应设备面板页面，家庭和设备的 Id 需要通过公版 SDK 接口获取

**接口说明**

打开设备面板

``` java
goPanelWithCheckAndTip(Activity activity, String devId)
```
**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| activity     | 当前页面 context |
| devId        | 设备Id 通过公版 SDK 接口获取                                |

**示例代码**
``` java
 AbsPanelCallerService service = MicroContext.getServiceManager().findServiceByInterface(AbsPanelCallerService.class.getName());
                service.goPanelWithCheckAndTip(PanelActivity.this, bean.devId);
```

### 跳转未实现路由地址

面板按钮点击无反应时请查看 logcat 日志内容，[通过路由回调跳转到实现页面](./access.md#application-初始化)

### 清除所有面板缓存

面板文件会存放在当前 app 存储目录下，若需要清理可调用此方法

**示例代码**

``` java
  ClearCacheService service = MicroContext.getServiceManager().findServiceByInterface(ClearCacheService.class.getName());
      if (service != null) {
        service.clearCache(PanelActivity.this);
      }
```

## 错误码

| 错误码 | 描述                  |
| ------ | ---------------------- |
| 1901   | 面板下载失败           |
| 1902   | 多语言包下载失败       |
| 1903   | 设备类型不支持         |
| 1904   | 群组内无设备           |
| 1905   | home id有误            |
| 1906   | 设备DeviceBean为null   |
| 1907   | 找不到可下载的面板资源 |
| 1908   | 固件版本不正确         |
| 1909   | 未知错误               |
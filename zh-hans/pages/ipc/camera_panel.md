# 摄像机业务包


 ## 功能介绍

涂鸦智能 Android IPC 业务包（ TuyaCameraPanelSDK ）是基于 [Tuya Smart Camera SDK](<https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/>) 开发的一系列摄像机功能相关的面板 SDK。主要包括以下功能：

- 预览面板，回放面板，云存储面板，消息中心面板，相册面板，设置面板。



 ## 业务包集成


### 集成准备

TuyaCameraPanelSDK 是基于 [涂鸦全屋智能 SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/) 3.17.6 版本上开发

集成 TuyaCameraPanelSDK 之前，需要做以下工作：

1. 集成涂鸦全屋智能 SDK，参考 [集成文档](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/Preparation.html)（包括申请 tuya App ID 和 App Secret、安全图片配置相关环境）
2. 完成摄像机设备配网


### 集成 SDK

1. 创建工程

   在 Android Studio 中建立你的工程

2. 根目录 build.gradle 配置

   在根目录 build.gradle 文件里添加需要集成的 dependencies 依赖库地址。

   ```java
   allprojects {
      repositories {
          //***** required start ****//
          maven { url "https://maven-other.tuya.com/repository/maven-releases/"}
            maven { url "https://maven-other.tuya.com/repository/maven-snapshots/" }
          maven { url 'https://jitpack.io' }
          //***** required end ****//
          google()
          jcenter()
          mavenCentral()
      }
   }
   
   buildscript {
      repositories {
          maven {url "https://maven-other.tuya.com/repository/maven-releases/"}
            maven { url "https://maven-other.tuya.com/repository/maven-snapshots/" }
          maven { url "https://jitpack.io" }
          mavenLocal()
          mavenCentral()
          google()
          jcenter()
      }
      dependencies {
          classpath 'com.android.tools.build:gradle:3.5.0'
          classpath 'com.antfortune.freeline:gradle:0.8.6'
          classpath "com.google.protobuf:protobuf-gradle-plugin:0.8.6"
          classpath 'org.apache.httpcomponents:httpclient:4.4.1'
          classpath 'com.tuya.android.module:tymodule-config:0.4.0'
      }
   }
   ```

3. 模块 build.gradle 配置

   在模块 build.gradle 文件里添加集成准备中下载的 dependencies 依赖及一些配置。

   ```java
   apply plugin: 'com.android.application'
   //apply plugin: 'tymodule-config'
   
   android {
      //... 其他默认配置
      defaultConfig {
          //...其他默认配置
          multiDexEnabled true
          ndk {
              abiFilters "armeabi-v7a", "arm64-v8a"
          }
      }
   
      //***** 强制配置 开始 ****//
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
          disable 'InvalidPackage'
      }
      //***** 强制配置 结束 ****//
   }
   
   dependencies {
      //***** 涂鸦摄像头面板SDK模块 必须依赖 开始 ****//
      implementation 'com.tuya.smart:tuyasmart-camera-panel-sdk:3.17.6-open.1'
      //涂鸦RN面板 依赖
      implementation 'com.tuya.smart:panel-sdk:0.5.6'
      // 商城组件 购买h5页面
      implementation 'com.tuya.smart:tuyasmart-webcontainer:3.12.6r125-h1'
      implementation 'com.tuya.smart:tuyasmart-appshell:3.10.0'
      //homesdk
      implementation "com.tuya.smart:tuyasmart-TuyaRNApi:5.26.13-open"
      implementation 'com.tuya.smart:tuyasmart:3.17.6'
      //*****  涂鸦摄像头面板SDK模块 必须依赖 结束 ****//
   
      //第三方组件依赖
      implementation 'com.readystatesoftware.systembartint:systembartint:1.0.3'
      implementation 'com.weigan:loopView:0.1.1'
      implementation 'com.facebook.infer.annotation:infer-annotation:0.11.2'
      implementation 'com.facebook.soloader:soloader:0.8.0'
      implementation 'com.facebook.fresco:fresco:1.3.0'
      implementation 'com.facebook.fresco:animated-gif:1.3.0'
      implementation "com.facebook.fresco:imagepipeline-okhttp3:1.3.0"
      implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
      implementation 'javax.inject:javax.inject:1'
      implementation 'com.alibaba:fastjson:1.1.67.android'
      implementation 'com.facebook.react:react-native:0.51.1.11'
      implementation 'com.airbnb.android:lottie:2.7.0'
      implementation 'org.apache.commons:commons-compress:1.9'
      implementation 'com.kyleduo.switchbutton:library:1.4.2'
      implementation 'com.github.PhilJay:MPAndroidChart:v3.0.3'
      implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'
      implementation 'io.reactivex.rxjava2:rxandroid:2.0.1'
      implementation 'io.reactivex.rxjava2:rxjava:2.1.7'
      implementation 'com.hannesdorfmann:adapterdelegates3:3.1.0'
      implementation 'com.android.support.constraint:constraint-layout:1.1.3'
      implementation 'com.android.support:multidex:1.0.3'
      implementation "com.android.support:appcompat-v7:28.0.0"
      implementation "com.android.support:cardview-v7:28.0.0"
      implementation "com.android.support:recyclerview-v7:28.0.0"
      implementation "com.android.support:support-annotations:28.0.0"
      implementation "com.android.support:support-compat:28.0.0"
      implementation "com.android.support:support-fragment:28.0.0"
      implementation "com.android.support:design:28.0.0"
      implementation "com.android.support:support-v4:28.0.0"
      //***** 必须依赖模块 结束 ****//
   
      //... 其他默认配置
   }
   ```
   
4. AndroidManifest.xml 配置

   在 AndroidManifest.xml 文件里配置 appkey 和 appSecret ，在配置相应的权限等

   ```java
      <!-- sdcard 权限-->
      <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
      <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
      <!-- 网络 权限-->
      <uses-permission android:name="android.permission.INTERNET" />
      <uses-permission android:name="android.permission.CHANGE_NETWORK_STATE" />
      <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
      <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
      <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
      <uses-permission android:name="android.permission.RECORD_AUDIO" />
      <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
      <uses-permission
          android:name="android.permission.ACCESS_FINE_LOCATION"
          android:required="false" />
      <uses-permission
          android:name="android.permission.WAKE_LOCK"
          android:required="false" />
      <uses-permission
          android:name="android.permission.CHANGE_WIFI_MULTICAST_STATE"
          android:required="false" />
      <uses-permission android:name="android.permission.VIBRATE" />
   
      <application >
   
          <meta-data
              android:name="TUYA_SMART_APPKEY"
              android:value="用户申请的应用Appkey" />
          <meta-data
              android:name="TUYA_SMART_SECRET"
              android:value="用户申请的应用密钥AppSecret" />
   
         <!--    ... -->
      </application>
   ```

5. Theme 配置

   ```xml
      <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
           <!-- default configuration start -->
            <item name="app_bg_color">@color/app_bg_color</item>
           <item name="list_line_color">@color/list_line_color</item>
           <item name="status_bg_color">@color/status_bg_color</item>
           <!-- default configuration end -->
       </style>
   ```
   
6. 混淆配置

   在 proguard-rules.pro 文件配置相应混淆配置

   ```java
      //...
      #fastJson
      -keep class com.alibaba.fastjson.**{*;}
      -dontwarn com.alibaba.fastjson.**
   
      #mqtt
      -keep class org.eclipse.paho.client.mqttv3.** { *; }
      -dontwarn org.eclipse.paho.client.mqttv3.**
   
      -dontwarn okio.**
      -dontwarn rx.**
      -dontwarn javax.annotation.**
      -keep class com.squareup.okhttp.** { *; }
      -keep interface com.squareup.okhttp.** { *; }
      -keep class okio.** { *; }
      -dontwarn com.squareup.okhttp.**
   
      -keep class com.tuya.**{*;}
      -dontwarn com.tuya.**
   ```

### SDK 初始化

在 Application 中初始化 TuyaCameraPanelSDK

**示例代码**

```java
public class TuyaSmartApp extends Application {
  @Override
  public void onCreate() {
    super.onCreate();

    // ... 确认 appkey ，appSecret存在
    TuyaWrapper.init(this);
    TuyaPanelSDK.init(this, "应用Appkey", "应用密钥AppSecret");
    TuyaCameraPanelSDK.init(this);
    // ... 其他
  }
}
```

> 注意事项 Appkey 和 AppSecret 需要配置 AndroidManifest.xml 文件里，或者在 build 环境里配置，也可以在代码里写入。



## 功能调用

TuyaCameraPanelSDK 是涂鸦智能摄像机各面板的调用对外暴露的接口，包含多个面板的跳转及自定义实现。

参考 demo 流程，配置成功摄像头，进行摄像头面板相关操作

> [TuyaCameraPanelSDK Demo](https://github.com/TuyaInc/tuyasmart_camera_panel_android_sdk)



### 设置家庭ID

跳转到摄像机面板前，需设置摄像机设备所在的家庭 Id，通过家庭 Id 和设备 Id 进入相应的面板页面

**接口说明**

设置摄像机当前所在的家庭ID

```java
TuyaCameraPanelSDK.setCurrentHomeId(homeId);
```

**参数说明**

| 参数   | 说明                                                         |
| :----- | :----------------------------------------------------------- |
| homeId | 设备所在的家庭ID，可通过 [涂鸦全屋智能 SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/) 家庭管理对应的接口获取 |

**示例代码**

```java
TuyaCameraPanelSDK.setCurrentHomeId(homeId);
```



### Native预览面板

摄像机原生预览面板，包括视频实时预览，清晰度切换，声音开关控制，截图，录制，对讲等功能，移动侦测，PTZ方向控制，收藏点添加/删除，巡航控制等

**面板类名**

CameraPanelActivity.class

**参数说明**

| 参数              | 说明                                      |
| :---------------- | :---------------------------------------- |
| extra_camera_uuid | 设备 id，通过设备家庭下面的设备列表下获取 |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putString("extra_camera_uuid", deviceId);
Intent intent = new Intent(context, CameraPanelActivity.class);
intent.putExtras(bundle);
context.startActivity(intent);
```



###  RN预览面板

摄像机 RN 预览面板，封装在涂鸦智能 PanelSDK 里面，参考 [设备面板 功能调用](./pages/panel/panel_device.md#功能调用)

**接口说明**

通过接口 gotoPanelViewControllerWithDevice() 可跳转到 RN 面板

```java
gotoPanelViewControllerWithDevice(Activity activity, long homeId, String devId, ITuyaPanelLoadCallback loadCallback);
```



### 回放面板

摄像机回放面板，展示的是保存在摄像机存储设备上的视频，包括视频回放，回放日期选择，视频随时间轴拖动播放，播放/暂停，声音控制，截图，录制等功能

**面板类名**

CameraPlaybackActivity.class

**参数说明**

| 参数              | 说明    |
| :---------------- | :------ |
| extra_camera_uuid | 设备 id |

**示例代码**

```java
Intent intent = new Intent(context, CameraPlaybackActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```



### 云存储面板

摄像机云存储面板，展示开通云存储功能之后录制保存的云端视频。包括视频云存储播放，云存储日期选择，视频随时间轴拖动播放，播放/暂停，声音控制，截图，录制等功能，移动侦测数据列表展示。

**面板类名**

CameraCloudActivity.class

**参数说明**

| 参数              | 说明                                      |
| :---------------- | :---------------------------------------- |
| extra_camera_uuid | 设备 id                                   |
| timeRangeBean     | 云存储最近一天的某条数据 position，可不传 |

**示例代码**

```java
Intent intent = new Intent(context, CameraCloudActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```



###  消息中心面板

摄像机消息中心面板，包括摄像机录制过程中产生的各类消息，按日期，消息类型进行展示，消息类别支持图片，视频，纯音频等，可进行预览及单条删除，全部删除操作。

**面板类名**

IPCCameraMessageCenterActivity.class

**参数说明**

| 参数              | 说明    |
| :---------------- | :------ |
| extra_camera_uuid | 设备 id |

**示例代码**

```java
Intent intent = new Intent(context, IPCCameraMessageCenterActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```



### 相册面板

摄像机相册面板，展示的是根据设备 id 所保存文件，这些文件是在摄像机预览/回放/云视频播放过程中，本地截图和录制视频。可进行预览，单条删除，全部删除等操作。

**面板类名**

LocalPhotoOrVideoActivity.class

**参数说明**

| 参数              | 说明    |
| :---------------- | :------ |
| extra_camera_uuid | 设备 id |

**示例代码**

```java
Intent intent = new Intent(context, LocalPhotoOrVideoActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```



### 门铃面板

摄像机门铃接听面板，显示推送过来的门铃消息界面，包括门铃基本信息，实时截图，接听，挂断功能；接听成功后会进入摄像机预览面板

**面板类名**

DoorBellCallingActivity.class

**参数说明**

| 参数              | 说明                                  |
| :---------------- | :------------------------------------ |
| extra_camera_uuid | 设备 id，一般通过推送过来的消息中提取 |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putString("devId", deviceId);
Intent intent = new Intent(context, DoorBellCallingActivity.class);
intent.putExtras(bundle);
context.startActivity(intent);
```



### 视频流门铃面板

摄像机视频流门铃接听面板，显示推送过来的实时视频流门铃消息界面，包括门铃状态信息，接听，挂断功能；门铃接听有时效性，停留时间 < 25s，接听成功后进行实时视频通话。

**面板类名**

DoorBellDirectCameraActivity.class

**参数说明**

| 参数                | 说明                                                         |
| :------------------ | :----------------------------------------------------------- |
| extra_camera_uuid   | 设备 id，一般通过推送过来的消息中提取                        |
| doorbell_start_time | long类型，表示按下门铃的开始时间，门铃接听面板从按下开始计时，停留时长为25s（实际 < 25s） |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putString("extra_camera_uuid", deviceId);
bundle.putLong("doorbell_start_time", startTime);
Intent intent = new Intent(context, DoorBellDirectCameraActivity.class);
intent.putExtras(bundle);
context.startActivity(intent);
```



### 设置面板

摄像机设置面板，可通过后台dp点配置展示（参考：[Tuya Smart Camera SDK - 设备功能](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/zh-hans/resource/camera_device_points) )，主要包含：

- 设备图标/名称
- 设备信息（所有者，ip 地址，设备 id，设备时区，信号强度等）
- 基础设置（隐私开关， 基本功能设置，红外夜视功能，画质调节 (亮度,对比度)，工作模式等）
- 高级设置（侦测报警设置，PIR 开关，电源管理设置，铃铛设置，蜂鸣器调节，视频布局，预支点设置等）
- 存储设置（sd 卡容量管理，格式化等）
- 增值服务（云存储购买等）
- 离线提醒
- 其他（常见问题反馈）
- 重启设备
- 移除设备

**面板类名**

CameraSettingActivity.class

**参数说明**

| 参数              | 说明    |
| :---------------- | :------ |
| extra_camera_uuid | 设备 id |

**示例代码**

```java
Intent intent = new Intent(context, CameraSettingActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```



###  面板自定义

TuyaCameraPanelSDK 支持用户自定义配置面板，用户若自行实现面板功能，可参考 [Tuya Smart Camera SDK](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/zh-hans/)

#### 自定义主题

主题包括黑色和白色2种，支持涂鸦智能摄像头的设置面板和本地相册面板

**接口说明**

用户可通过 TuyaCameraPanelSDK.setTheme() 方法来设置主题

```java
// themeId 1：黑色主题，2：白色主题
TuyaCameraPanelSDK.setTheme(themeId)；
```

**参数说明**

| 参数    | 说明                             |
| :------ | :------------------------------- |
| themeId | int 型：1，黑色主题；2，白色主题 |

**示例代码**

```java
TuyaCameraPanelSDK.setTheme(1)；
```

> 仅支持设置/本地相册面板

#### 自定义回放

  若提供的回放面板不满足用户需求，用户可自行实现回放面板，参考 [Tuya Smart Camera SDK - 回放](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/zh-hans/resource/PlaybackProcess.html)

  **接口说明**

  设置 PlaybackPanelListener 监听，跳转到自定义面板去实现回放

  ```java
  TuyaCameraPanelSDK.setCustomPlaybackPanelListener(PlaybackPanelListener playbackListener);
  ```

  **参数说明**

| 参数             | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| playbackListener | PlaybackPanelListener 接口，设置此监听，可自定义回放面板；不设置则跳转到默认涂鸦摄像机回放面板 |

  **示例代码**

  ```java
  TuyaCameraPanelSDK.setCustomPlaybackPanelListener(new PlaybackPanelListener() {
      @Override
      public void onPlaybackPanelClick(String deviceId) {
          L.d(TAG, "回放面板 -自定义实现" + deviceId);
      }
  });
  ```

#### 自定义云存储

  用户自定义去实现云存储面板，参考 [Tuya Smart Camera SDK - 云存储](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/zh-hans/resource/CloudStorageProcess.html)

  **接口说明**

  设置 CloudStoragePanelListener 监听，自定义面板去实现云存储视频播放

  ```java
  TuyaCameraPanelSDK.setCustomCloudStoragePanelListener(CloudStoragePanelListener cloudStoragePanelListener);
  ```

  **参数说明**

| 参数                      | 说明                                                         |
| :------------------------ | :----------------------------------------------------------- |
| cloudStoragePanelListener | CloudStoragePanelListener 接口，设置此监听，可自定义云存储面板；不设置则跳转到默认涂鸦摄像机云存储面板 |

  **示例代码**

  ```java
  TuyaCameraPanelSDK.setCustomCloudStoragePanelListener(new CloudStoragePanelListener() {
      @Override
      public void onCloudStoragePanelClick(String deviceId) {
          L.d(TAG, "云存储面板 -自定义实现" + deviceId);
      }
  });
  ```

#### 自定义相册

  用户自定义实现本地相册

  **接口说明**

  设置 AlbumPanelListener 监听，自定义面板去实现当前 deviceId 在预览，回放，云存储视频播放中保存的图片，视频信息展示，本地路径为：

  - 截图路径：Environment.getExternalStorageDirectory().absolutePath + "/Camera/" + deviceId + "/"；
  - 录制路径：Environment.getExternalStorageDirectory().absolutePath + "/Camera/Thumbnail/" + deviceId + "/"

  ```java
  TuyaCameraPanelSDK.setCustomAlbumPanelListener(AlbumPanelListener albumPanelListener);
  ```

  **参数说明**

| 参数               | 说明                                                         |
| :----------------- | :----------------------------------------------------------- |
| albumPanelListener | AlbumPanelListener 接口，设置此监听，可自定义相册面板；不设置则跳转到默认涂鸦摄像机相册面板 |

  **示例代码**

  ```java
  TuyaCameraPanelSDK.setCustomAlbumPanelListener(new AlbumPanelListener() {
      @Override
      public void onAlbumPanelClick(String deviceId) {
          L.d(TAG, "相册面板 -自定义实现" + deviceId);
      }
  });
  ```

#### 自定义消息中心

  用户自定义实现消息中心面板，参考 [Tuya Smart Camera SDK - 消息中心](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/zh-hans/resource/message_center_list.html)

  **接口说明**

  设置 MessagePanelListener 监听，自定义面板去实现消息中心数据的展示

  ```java
  TuyaCameraPanelSDK.setCustomMessagePanelListener(MessagePanelListener messagePanelListener);
  ```

  **参数说明**

| 参数                 | 说明                                                         |
| :------------------- | :----------------------------------------------------------- |
| messagePanelListener | MessagePanelListener 接口，设置此监听，可自定义消息中心；不设置则跳转到默认涂鸦摄像机消息中心面板 |

  **示例代码**

  ```java
  TuyaCameraPanelSDK.setCustomMessagePanelListener(new MessagePanelListener() {
      @Override
      public void onMessagePanelClick(String deviceId) {
          L.d(TAG, "消息中心面板 -自定义实现" + deviceId);
      }
  });
  ```

#### 自定义设置

  用户自定义实现设置模块，参考 [Tuya Smart Camera SDK - 设备功能](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/zh-hans/resource/camera_device_points.html)

  **接口说明**

  设置 SettingListener 监听，实现自定义面板设置功能，入口在预览面板右上角

  ```java
  TuyaCameraPanelSDK.setCustomSettingListener(SettingListener settingListener);
  ```

  **参数说明**

| 参数            | 说明                                                         |
| :-------------- | :----------------------------------------------------------- |
| settingListener | SettingListener 接口，设置此监听，可自定义云存储面板；可自定义设置面板，不设置则调用默认涂鸦摄像机的设置面板 |

  **示例代码**

  ```java
  TuyaCameraPanelSDK.setCustomSettingListener(new SettingListener() {
      @Override
      public void onSettingClick() {
          L.d(TAG, "自定义设置");
      }
  });
  ```

#### 设置面板 - 设备信息

  用户在涂鸦智能摄像机设置面板内去自定义实现设备图标，名称的修改

  **接口说明**

  设置 DeviceInfoListener 监听，自定义面板去实现设备信息修改

  ```java
  TuyaCameraPanelSDK.setCustomDeviceInfoListener(DeviceInfoListener deviceInfoListener);
  ```

  **参数说明**

| 参数               | 说明                                                         |
| :----------------- | :----------------------------------------------------------- |
| deviceInfoListener | DeviceInfoListener 接口，设置此监听，可自定义设置实现 面板设置-设备信息功能 |

  **示例代码**

  ```java
  TuyaCameraPanelSDK.setCustomDeviceInfoListener(new DeviceInfoListener() {
      @Override
      public void onDeviceInfoClick(String deviceId) {
          L.d(TAG, "设备信息 -自定义实现");
      }
  });
  ```

#### 设置面板 - 意见反馈

  用户在涂鸦智能摄像机设置面板内去自定义实现意见反馈

  **接口说明**

  设置 FeedbackListener 监听，自定义面板去实现面板设置-意见反馈功能

  ```java
  TuyaCameraPanelSDK.setCustomFeedbackListener(FeedbackListener feedbackListener);
  ```

  **参数说明**

| 参数             | 说明                                                         |
| :--------------- | :----------------------------------------------------------- |
| feedbackListener | FeedbackListener 接口，设置此监听，可自定义实现 意见反馈，不设置该监听则面板上将不显示此项 |

  **示例代码**

  ```java
  TuyaCameraPanelSDK.setCustomFeedbackListener(new FeedbackListener() {
      @Override
      public void onFeedbackClick(String deviceId) {
          L.d(TAG, "意见反馈 -自定义实现");
      }
  });
  ```



### 修改设备信息

#### 设备名称

  用户可自行修改摄像机设备名称

  **接口说明**

  调用 TuyaHomeSdk 下面的 renameDevice() 进行设备名称的修改

  ```java
  TuyaHomeSdk.newDeviceInstance(deviceId).renameDevice(String deviceName, IResultCallback callback);
  ```

  **参数说明**

| 参数       | 说明                                            |
| :--------- | :---------------------------------------------- |
| deviceId   | 设备 id                                         |
| deviceName | 设备重命名的名称                                |
| callback   | IResultCallback 接口，设备重命名成功/失败的回调 |

  **示例代码**

  ```java
  /**
   * 修改设备名称
   * @param context
   * @param deviceId      设备id
   * @param deviceName    设备重命名名称
   */
  public void renameDevice(final Context context, String deviceId, String deviceName) {
      ITuyaDevice mDevice = TuyaHomeSdk.newDeviceInstance(deviceId);
      mDevice.renameDevice(deviceName, new IResultCallback() {
          @Override
          public void onError(String code, String error) {
  
          }
  
          @Override
          public void onSuccess() {
  
          }
      });
  }
  ```

#### 设备图标

  用户可自行修改摄像机设备图标

  **接口说明**

  调用 TuyaHomeSdk 下面的 modifyDeviceImg() 进行设备图标的修改

  ```java
  DeviceInfoRepository deviceInfoRepository = new DeviceInfoRepositoryImpl(context);
  ModifyDevInfoInteractor mModifyDevInfoInteractor = new ModifyDevInfoInteractorImpl(deviceInfoRepository);
  mModifyDevInfoInteractor.modifyDeviceImg( deviceId,  deviceName, imageFile,  callback);
  ```

  **参数说明**

| 参数       | 说明                                                         |
| :--------- | :----------------------------------------------------------- |
| deviceId   | 设备 id                                                      |
| imageFile  | File 类型，表示待上传的图片文件                              |
| deviceName | 设备名称，通过公版 TuyaHomeSdk 的 DeviceBean 获取            |
| callback   | ModifyDevInfoInteractor.ModifyDeviceImgCallback 接口，上传的图片文件成功/失败的回调 |

  **示例代码**

  ```java
  /**
   * 修改设备头像
   *
   * @param context
   * @param deviceId     设备id
   * @param iconFilePath 待上传的设备头像地址
   */ 
  public void uploadIcon(final Context context, String deviceId, String iconFilePath) {
      DeviceBean deviceBean = TuyaHomeSdk.getDataInstance().getDeviceBean(deviceId);
      String panelName = "";
      if (deviceBean != null) {
          panelName = deviceBean.getName();
      }
      DeviceInfoRepository deviceInfoRepository = new DeviceInfoRepositoryImpl(context);
      ModifyDevInfoInteractor mModifyDevInfoInteractor = new ModifyDevInfoInteractorImpl(deviceInfoRepository);
      ProgressUtils.showLoadingViewFullPage(context);
      mModifyDevInfoInteractor.modifyDeviceImg(deviceId, panelName, new File(iconFilePath),
              new ModifyDevInfoInteractor.ModifyDeviceImgCallback() {
                  @Override
                  public void onModifyDeviceImgSuccess(String url) {
  
                      ProgressUtils.hideLoadingViewFullPage();
                  }
  
                  @Override
                  public void onModifyDeviceImgFailure() {
  
                      ProgressUtils.hideLoadingViewFullPage();
                  }
              });
  }
  ```



### 消息推送辅助 43协议

在APP进程活着的情况下，为提高推送消息的到达及时性和成功率，涂鸦智能摄像机开放了消息推送辅助协议。

- 用户自行实现接入推送渠道，可参考 [涂鸦全屋智能 SDK 系统推送消息](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/MessagePush.html)
- 注册涂鸦推送消息监听，获取回调上来的推送消息，进行后续处理

消息体格式定义示例

```json
{
"a": "view",
"c": "action",
"cc": "低功耗智能摄像机 ,someone is ringing the bell.",
"ct": "fcm You have a visitor",
"devId": "6cfaf335a8d6e752e0wrpy",
"msgId": "4da4dcf61573555995",
"p": {
"media": 13
},
"specialChannel": false,
"ts": "1573555995000",
"type": "doorbell"
}
```

**注册和注销监听**

在账号登录成功后注册,在账号退出时进行注销

**接口说明**

涂鸦推送辅助协议需在账号登陆成功后注册监听，在账号退出时进行注销

```java
//注册涂鸦推送消息监听
TuyaHomeSdk.getCameraInstance().registerCameraPushListener(ITuyaGetBeanCallback<CameraPushDataBean> callback)

//注销涂鸦推送消息监听
TuyaHomeSdk.getCameraInstance().unRegisterCameraPushListener(ITuyaGetBeanCallback<CameraPushDataBean> callback)；
```

**参数说明**

| 参数     | 说明                                                         |
| :------- | :----------------------------------------------------------- |
| callback | ITuyaGetBeanCallback 接口，监听回调推送消息，CameraPushDataBean 为推送消息封装 |

**CameraPushDataBean 数据模型**

| 字段      | 类型    | 描述       |
| :-------- | :------ | :--------- |
| devId     | String  | 设备id     |
| timestamp | Integer | 消息时间戳 |
| etype     | String  | 消息类型   |
| edata     | String  | 消息id     |

**示例代码**

```java
private ITuyaHomeCamera homeCamera;
private static ITuyaGetBeanCallback<CameraPushDataBean> mTuyaGetBeanCallback = new ITuyaGetBeanCallback<CameraPushDataBean>() {
    @Override
    public void onResult(CameraPushDataBean o) {
        L.d(TAG, "onMqtt_43_Result on callback");
        L.d(TAG, "消息时间戳：timestamp=" + o.getTimestamp());
        L.d(TAG, "设备id：devid=" + o.getDevId());
        L.d(TAG, "消息id：msgid=" + o.getEdata());
        L.d(TAG, "消息类型：etype=" + o.getEtype());
    }
};

/**
 * 在账号登陆成功之后注册
 */
public void registerCameraPushListener() {
    homeCamera = TuyaHomeSdk.getCameraInstance();
    if (homeCamera != null) {
        homeCamera.registerCameraPushListener(mTuyaGetBeanCallback);
    }
}

/**
 * 在账号注销之后进行反注册
 */
public void unRegisterCameraPushListener() {
    if (homeCamera != null) {
        homeCamera.unRegisterCameraPushListener(mTuyaGetBeanCallback);
    }
}
```

> **注意**:
>
> 1. 当app进程被杀死的时候，该监听无效。在app登录成功之后，注册监听；app退出登录，取消监听
> 2. 避免重复的推送消息（用户自有渠道推送、涂鸦辅助协议产生），可通过消息id，消息类型，消息到达时间来进行过滤



### 面板多语言配置

下载SDK提供的 [多语言包](https://github.com/TuyaInc/tuya_smart_bizbundle_language)，选择需要的语言包配置到 res 目录下
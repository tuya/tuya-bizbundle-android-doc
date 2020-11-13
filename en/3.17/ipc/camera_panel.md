# IPC Device bizBundle

## Features

TuyaSmart Android IPC biz Bundle (referred to as: TuyaCameraPanelSDK ) is a series of panel SDKs related to camera functions developed based on [Tuya Smart Camera SDK](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/). It mainly includes the following functions:

-Preview panel, playback panel, cloud storage panel, message center panel, album panel, settings panel.



## Integrate

### Preparation

TuyaCameraPanelSDK is based on TuyaHomeSdk 3.17.6 version

Before integrating TuyaCameraPanelSDK, you need to do the following:

1. Integration of TuyaHomeSdk is completed, refer to [Tuya Smart Home SDK Documents](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/) (including applying for Tuya App ID and App Secret, security picture configuration related environment)
2. Camera equipment completes network distribution

###  Integrated SDK

1. Build your project in Android Studio

2.  Configure build.gradle in the root directory

   Add the addresses of dependencies to be integrated in the build.gradle file in the root directory.

   ```java
   allprojects {
      repositories {
          //***** required start ****//
          maven { url 'https://jitpack.io' }
          //***** required end ****//
          google()
          jcenter()
          mavenCentral()
      }
   }
   
   buildscript {
      repositories {
          maven { url "https://jitpack.io" }
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

3. Configure build.gradle in the module directory

   In the module's build.gradle file, add the dependencies and some configurations in the integration preparation.

   ```java
   apply plugin: 'com.android.application'
   
   android {
      //... 
      defaultConfig {
          //...
          multiDexEnabled true
          ndk {
              abiFilters "armeabi-v7a", "arm64-v8a"
          }
      }
   
      //***** start ****//
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
      //***** end ****//
   }
   
   dependencies {
      //***** start ****//
      implementation 'com.tuya.smart:tuyasmart-camera-panel-sdk:3.17.6-open.1'
      //RN
      implementation 'com.tuya.smart:panel-sdk:0.5.6'
      //homesdk
      implementation 'com.tuya.smart:tuyasmart:3.17.6'
       
      implementation 'com.tuya.smart:tuyasmart-imagepipeline-okhttp3:0.0.1'
      
      implementation 'com.tuya.smart:tuyasmart-webcontainer:3.12.6r125-h1'
      implementation 'com.tuya.smart:tuyasmart-appshell:3.10.0'
      
      implementation 'com.tuya.smart:tuyasmart-rpc:3.12.0r123'
      
      implementation 'com.tuya.smart:tuyasmart-video:3.12.6r125'
   
      implementation 'com.tuya.android.module:tymodule-annotation:0.0.7.2'
      implementation 'com.tuya.smart:tuyasmart-wkvideoplayer:1.0.0'
      //*****  end ****//
   
      //third
      implementation 'com.weigan:loopView:0.1.1'
      implementation 'com.facebook.infer.annotation:infer-annotation:0.11.2'
      implementation 'com.facebook.soloader:soloader:0.8.0'
      implementation 'com.facebook.fresco:fresco:1.3.0'
      implementation 'com.facebook.fresco:animated-gif:1.3.0'
      implementation "com.facebook.fresco:imagepipeline-okhttp3:1.3.0"
      implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
      implementation 'javax.inject:javax.inject:1'
      implementation 'com.alibaba:fastjson:1.1.67.android'
      implementation 'com.tuya.smart:react-native:0.51.1.11'
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
      //***** end ****//
   
      //...
   }
   ```

5. Configure AndroidManifest.xml

   Configure appkey and appSecret in AndroidManifest.xml file, configure corresponding permissions, etc.

   ```java
      <!-- sdcard -->
      <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
      <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
      <!-- net -->
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
              android:value="Appkey" />
          <meta-data
              android:name="TUYA_SMART_SECRET"
              android:value="AppSecret" />
   
         <!--    ... -->
      </application>
   ```

5.  Configure theme

      ```xml
      <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
              <!-- default configuration start -->
               <item name="app_bg_color">@color/app_bg_color</item>
              <item name="list_line_color">@color/list_line_color</item>
              <item name="status_bg_color">@color/status_bg_color</item>
              <!-- default configuration end -->
          </style>
      ```
   
6. Configure proguard-rules.pro

   Configure in the proguard-rules.pro file

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

### Initialize the SDK

Initialize TuyaCameraPanelSDK in Application

**Example**

```java
public class TuyaSmartApp extends Application {
  @Override
  public void onCreate() {
    super.onCreate();

    // ... Confirm that appkey and appSecret exist
    TuyaWrapper.init(this);
    TuyaPanelSDK.init(this, "Appkey", "AppSecret");
    TuyaCameraPanelSDK.init(this);
    // ... 
  }
}
```

> Note: Appkey and AppSecret need to be configured in the AndroidManifest.xml file, or in the build environment, or written in the code.



## Function

TuyaCameraPanelSDK is an external public interface of Tuya Smart Camera Panel SDK, which includes jump and custom implementation of multiple panels.

Please refer to the demo procedure to configure a successful camera and perform operations related to the camera panel.

> [TuyaCameraPanelSDK Demo](https://github.com/TuyaInc/tuyasmart_camera_panel_android_sdk)

### Set HomeId

Before jumping to the camera panel, you need to set the home Id where the camera device is located. Enter the corresponding panel page through the home Id and device Id

**Declaration**

Set the homeId where the camera is currently located

```java
TuyaCameraPanelSDK.setCurrentHomeId(homeId);
```

**Parameters**

| Parameter | Description                                                  |
| :-------- | :----------------------------------------------------------- |
| homeId    | the homeId where the device is located, which can be obtained through the [TuyaSmartHomeSDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/) - Home Management interface |

**Example**

```java
TuyaCameraPanelSDK.setCurrentHomeId(homeId);
```

### Native Preview Panel

Camera native preview panel, including real-time video preview, sharpness switch, sound switch control, screenshot, recording, intercom and other functions, motion detection, PTZ direction control, favorite point addition / deletion, cruise control, etc.

**Panel Class Name**

CameraPanelActivity.class

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | device id   |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putString("extra_camera_uuid", deviceId);
Intent intent = new Intent(context, CameraPanelActivity.class);
intent.putExtras(bundle);
context.startActivity(intent);
```



###  RN Preview Panel

Camera RN preview panel, packaged in TuyaPanelSDK, refer to [Device Control](./pages/panel/panel_device.md#function-call)

**Declaration**

TuyaPanelSDK.getPanelInstance().gotoPanelViewControllerWithDevice() to jump to the RN panel

```java
gotoPanelViewControllerWithDevice(Activity activity, long homeId, String deviceId, ITuyaPanelLoadCallback loadCallback);
```

### Playback Panel

The camera playback panel displays the video saved on the camera storage device, including video playback, playback date selection, video drag and play with time axis, play / pause, sound control, screenshot, recording and other functions

**Panel Class Name**

CameraPlaybackActivity.class

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | Device id   |

**Example**

```java
Intent intent = new Intent(context, CameraPlaybackActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```

### Cloud Storage Panel

The camera's cloud storage panel displays recorded cloud videos recorded after the cloud storage function is enabled. Including video cloud storage playback, cloud storage date selection, video drag and play with time axis, play / pause, sound control, screenshot, recording and other functions, and motion detection data list display.

**Panel Class Name**

CameraCloudActivity.class

**Parameters**

| Parameter         | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| extra_camera_uuid | Device id                                                    |
| timeRangeBean     | Cloud storage of a piece of data's position from the last day ,not necessary |

**Example**

```java
Intent intent = new Intent(context, CameraCloudActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```

###  Message-Center Panel

The camera message center panel includes all kinds of messages generated during the recording process of the camera. It is displayed by date and message type. The message category supports pictures, videos, and pure audio. It can be previewed and deleted.

**Panel Class Name**

IPCCameraMessageCenterActivity.class

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | device id   |

**Example**

```java
Intent intent = new Intent(context, IPCCameraMessageCenterActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```



### Album Panel

The camera album panel shows the files saved according to the device id. These files are local screenshots and recorded videos during the camera preview / playback / cloud video playback process. Can perform preview, single delete, delete all operations.

**Panel Class Name**

LocalPhotoOrVideoActivity.class

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | Device id   |

**Example**

```java
Intent intent = new Intent(context, LocalPhotoOrVideoActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```



### Doorbell Panel

The camera doorbell answer panel displays the doorbell message interface that is pushed over, including the basic information of the doorbell, real-time screenshot, answer, and hang up functions; it will enter the camera preview panel after the answer is successful

**Panel Class Name**

DoorBellCallingActivity.class

**Parameters**

| Parameter         | Description                                                |
| :---------------- | :--------------------------------------------------------- |
| extra_camera_uuid | device id，It is usually extracted from the message pushed |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putString("devId", deviceId);
Intent intent = new Intent(context, DoorBellCallingActivity.class);
intent.putExtras(bundle);
context.startActivity(intent);
```



### Video Doorbell Panel

The camera video stream doorbell answer panel displays the real-time video stream doorbell message interface that is pushed over, including doorbell status information, answering, and hang-up functions; the doorbell answering timeliness, dwell time <25s, and real-time video call after successful answer.

**Panel Class Name**

DoorBellDirectCameraActivity.class

**Parameters**

| Parameter           | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| extra_camera_uuid   | device id,it is usually extracted from the message pushed    |
| doorbell_start_time | Long type, which indicates the start time of pressing the doorbell. The doorbell answering panel counts from the time of pressing, and the stay time is 25s (actually <25s) |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putString("extra_camera_uuid", deviceId);
bundle.putLong("doorbell_start_time", startTime);
Intent intent = new Intent(context, DoorBellDirectCameraActivity.class);
intent.putExtras(bundle);
context.startActivity(intent);
```



### Set Panel

Camera settings panel, which can be configured and displayed through the background dp point（[Tuya Smart Camera SDK - Equipment function point](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/en/resource/camera_device_points))，include：

- device name and icon
- device information(owner,ip,device id, device time zone，wifi signal)
- base setting(Privacy switch, basic function settings, infrared night vision function, picture quality adjustment (brightness, contrast), working mode, etc.)
- Advanced settings (detection alarm setting, PIR switch, power management setting, bell setting, siren adjustment, video layout, pre-point setting, etc.)
- Storage settings (SD card capacity management, formatting, etc.)
- Value-added services (cloud storage purchase, etc.)
- Offline reminder
- Other (Frequently Asked Questions)
- reboot device
- Remove device

**Panel Class Name**

CameraSettingActivity.class

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | device id   |

**Example**

```java
Intent intent = new Intent(context, CameraSettingActivity.class);
intent.putExtra("extra_camera_uuid", deviceId);
context.startActivity(intent);
```



###  PanelSDK customization

TuyaCameraPanelSDK supports user-defined configuration panel. If users implement the panel function by themselves, please refer to [Tuya Smart Camera SDK](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/en/)

#### Theme customization

  Themes include two types: black and white, support the settings panel and local album panel of Tuya Smart Camera

  **Declaration**

  Users can set the theme through TuyaCameraPanelSDK.setTheme () method

  ```java
  // themeId 1：black，2：white
  TuyaCameraPanelSDK.setTheme(themeId)；
  ```

  **Parameters**

| Parameter | Description          |
| :-------- | :------------------- |
| themeId   | int: 1,black;2,white |

  **Example**

  ```java
  TuyaCameraPanelSDK.setTheme(1)；
  ```

  > Only support settings / local album panel

#### Playback customization

  If the provided playback panel does not meet user needs, developers can implement the playback panel by themselves,refer to [Tuya Smart Camera SDK - Playback](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/en/resource/PlaybackProcess.html)

  **Declaration**

  Set PlaybackPanelListener to jump to the custom panel to implement playback

  ```java
  TuyaCameraPanelSDK.setCustomPlaybackPanelListener(PlaybackPanelListener playbackListener);
  ```

  **Parameters**

| Parameter        | Description                                                  |
| :--------------- | :----------------------------------------------------------- |
| playbackListener | PlaybackPanelListener interface, set this monitor to customize the playback panel; if not set, jump to the default Tuya camera playback panel |

  **Example**

  ```java
  TuyaCameraPanelSDK.setCustomPlaybackPanelListener(new PlaybackPanelListener() {
      @Override
      public void onPlaybackPanelClick(String deviceId) {
          L.d(TAG, "Customize Playback " + deviceId);
      }
  });
  ```

#### Cloud Storage customization

  User-defined cloud storage,refer to [Tuya Smart Camera SDK - CloudStorage](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/en/resource/CloudStorageProcess.html)

  **Declaration**

  Set up CloudStoragePanelListener, customize the panel to implement cloud storage video playback.

  ```java
  TuyaCameraPanelSDK.setCustomCloudStoragePanelListener(CloudStoragePanelListener cloudStoragePanelListener);
  ```

  **Parameters**

| Parameter                 | Description                                                  |
| :------------------------ | :----------------------------------------------------------- |
| cloudStoragePanelListener | CloudStoragePanelListener interface, set this listener to customize the cloud storage panel; if not set, skip to the default Tuya camera cloud storage panel |

  **Example**

  ```java
  TuyaCameraPanelSDK.setCustomCloudStoragePanelListener(new CloudStoragePanelListener() {
      @Override
      public void onCloudStoragePanelClick(String deviceId) {
          L.d(TAG, "Customize cloud storage " + deviceId);
      }
  });
  ```

#### Album customization

  User-defined local photo album

  **Declaration**

  Set AlbumPanelListener, customize the panel to achieve the current deviceId image preview, playback, cloud storage video playback, video information display,local storage path:

  - Screenshot：Environment.getExternalStorageDirectory().absolutePath + "/Camera/" + deviceId + "/"；
  - Record：Environment.getExternalStorageDirectory().absolutePath + "/Camera/Thumbnail/" + deviceId + "/"

  ```java
  TuyaCameraPanelSDK.setCustomAlbumPanelListener(AlbumPanelListener albumPanelListener);
  ```

  **Parameters**

| Parameter          | Description                                                  |
| :----------------- | :----------------------------------------------------------- |
| albumPanelListener | AlbumPanelListener interface, set this listener to customize the album panel; if not set, skip to the default Tuya camera album panel |

  **Example**

  ```java
  TuyaCameraPanelSDK.setCustomAlbumPanelListener(new AlbumPanelListener() {
      @Override
      public void onAlbumPanelClick(String deviceId) {
          L.d(TAG, "customize album" + deviceId);
      }
  });
  ```

#### Message-Center customization

  User-defined implementation of message center panel,reference [Tuya Smart Camera SDK - Message Center](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/en/resource/message_center_list.html)

  **Declaration**

  Set up MessagePanelListener and customize the panel to display message center data

  ```java
  TuyaCameraPanelSDK.setCustomMessagePanelListener(MessagePanelListener messagePanelListener);
  ```

  **Parameters**

| Parameter            | Description                                                  |
| :------------------- | :----------------------------------------------------------- |
| messagePanelListener | MessagePanelListener interface, set this listener to customize the message center; if not set, skip to the default graffiti camera message center panel |

  **Example**

  ```java
  TuyaCameraPanelSDK.setCustomMessagePanelListener(new MessagePanelListener() {
      @Override
      public void onMessagePanelClick(String deviceId) {
          L.d(TAG, "Customize Message Center " + deviceId);
      }
  });
  ```

#### Setting customization

  User-defined implementation setting module, refer to [Tuya Smart Camera SDK - Set](https://tuyainc.github.io/tuyasmart_camera_android_sdk_doc/en/resource/camera_device_points.html)

  **Declaration**

  Set SettingListener to implement the custom panel setting function. The entrance is in the upper right corner of the preview panel.

  ```java
  TuyaCameraPanelSDK.setCustomSettingListener(SettingListener settingListener);
  ```

  **Parameters**

| Parameter       | Description                                                  |
| :-------------- | :----------------------------------------------------------- |
| settingListener | SettingListener interface, set this listener, you can customize the cloud storage panel; you can customize the settings panel, if not set, call the settings panel of the default Tuya camera |

  **Example**

  ```java
  TuyaCameraPanelSDK.setCustomSettingListener(new SettingListener() {
      @Override
      public void onSettingClick() {
          L.d(TAG, "Customize Set");
      }
  });
  ```

#### Setting - Device infomation customization

  The developer can customize the device icon and name in the Tuya smart camera settings panel.

  **Declaration**

  Set DeviceInfoListener and customize the panel to modify device information.

  ```java
  TuyaCameraPanelSDK.setCustomDeviceInfoListener(DeviceInfoListener deviceInfoListener);
  ```

  **Parameters**

| Parameter          | Description                                                  |
| :----------------- | :----------------------------------------------------------- |
| deviceInfoListener | DeviceInfoListener interface, set this listener, can be customized to achieve panel settings-device information function |

  **Example**

  ```java
  TuyaCameraPanelSDK.setCustomDeviceInfoListener(new DeviceInfoListener() {
      @Override
      public void onDeviceInfoClick(String deviceId) {
          L.d(TAG, "Customize device information");
      }
  });
  ```

#### Setting - Feedback customization

  Developers can customize feedback in Tuya Smart Camera Settings panel to implement feedback

  **Declaration**

  Set FeedbackListener to customize the panel to implement panel settings-feedback function

  ```java
  TuyaCameraPanelSDK.setCustomFeedbackListener(FeedbackListener feedbackListener);
  ```

  **Parameters**

| Parameter        | Description                                                  |
| :--------------- | :----------------------------------------------------------- |
| feedbackListener | FeedbackListener interface. Set this listener to customize feedback. If you do not set this listener, this item will not be displayed |

  **Example**

  ```java
  TuyaCameraPanelSDK.setCustomFeedbackListener(new FeedbackListener() {
      @Override
      public void onFeedbackClick(String deviceId) {
          L.d(TAG, "Customize feedback");
      }
  });
  ```



### Modify Device information

#### Modify device name

  Developers can modify camera device name

  **Declaration**

  Call renameDevice () under TuyaHomeSdk to modify the device name

  ```java
  TuyaHomeSdk.newDeviceInstance(deviceId).renameDevice(String deviceName, IResultCallback callback);
  ```

  **Parameters**

| Parameter  | Description                                                  |
| :--------- | :----------------------------------------------------------- |
| deviceId   | device id                                                    |
| deviceName | device renamed name                                          |
| callback   | IResultCallback interface，device rename success / failure callback |

  **Example**

  ```java
  /**
   * @param context
   * @param deviceId      
   * @param deviceName   
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

#### Modify device icon

  Users can modify the camera icon by themselves.

  **Declaration**

  Call modifyDeviceImg () under TuyaHomeSdk to modify the device icon

  ```java
  DeviceInfoRepository deviceInfoRepository = new DeviceInfoRepositoryImpl(context);
  ModifyDevInfoInteractor mModifyDevInfoInteractor = new ModifyDevInfoInteractorImpl(deviceInfoRepository);
  mModifyDevInfoInteractor.modifyDeviceImg( deviceId,  deviceName, imageFile,  callback);
  ```

  **Parameters**

| Parameter  | Description                                                  |
| :--------- | :----------------------------------------------------------- |
| deviceId   | device id                                                    |
| imageFile  | File , Indicates the image file to be uploaded               |
| deviceName | device name，Available through DeviceBean in the TuyaHomeSdk |
| callback   | ModifyDevInfoInteractor.ModifyDeviceImgCallback interface,uploaded image file success / failure callback |

  **Example**

  ```java
  /**
   * modify icon
   *
   * @param context
   * @param deviceId     
   * @param iconFilePath
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



### Tuya Message Push Assistant Protocol 43

With the APP process alive, in order to improve the timeliness and success rate of push messages, Tuya Smart Camera has opened a message push assistant protocol.

- Users implement their own access to push channels, refer to [Tuya Smart Home SDK system push message](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/MessagePush.html)
- Register for Tuya push message monitoring, get the push message from the callback, and follow up

Message body format definition example:

```json
{
"a": "view",
"c": "action",
"cc": "someone is ringing the bell.",
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

**Register and logout listening**

Register after the account is successfully logged in, and log out when the account is logged out

**Declaration**

The Tuya push assistant protocol needs to register for monitoring after the account login is successful, and log out when the account is logged out.

**Declaration**

The Tuya push assistant protocol needs to register for monitoring after the account login is successful, and log out when the account is logged out.

```java
//register
TuyaHomeSdk.getCameraInstance().registerCameraPushListener(ITuyaGetBeanCallback<CameraPushDataBean> callback)

//logout
TuyaHomeSdk.getCameraInstance().unRegisterCameraPushListener(ITuyaGetBeanCallback<CameraPushDataBean> callback)；
```

**Parameters**

| Parameter | Description                                                  |
| :-------- | :----------------------------------------------------------- |
| callback  | ITuyaGetBeanCallback interface, monitor callback push messages, CameraPushDataBean encapsulates push messages |

**CameraPushDataBean Data Model**

| Field     | Type    | Description               |
| :-------- | :------ | :------------------------ |
| devId     | String  | device id                 |
| timestamp | Integer | timestamp of push message |
| etype     | String  | message type              |
| edata     | String  | message id                |

**Example**

```java
private ITuyaHomeCamera homeCamera;
private static ITuyaGetBeanCallback<CameraPushDataBean> mTuyaGetBeanCallback = new ITuyaGetBeanCallback<CameraPushDataBean>() {
    @Override
    public void onResult(CameraPushDataBean o) {
        L.d(TAG, "onMqtt_43_Result on callback");
        L.d(TAG, "timestamp=" + o.getTimestamp());
        L.d(TAG, "devid=" + o.getDevId());
        L.d(TAG, "msgid=" + o.getEdata());
        L.d(TAG, "msgtype=" + o.getEtype());
    }
};

/**
 * Register listening after successful account login
 */
public void registerCameraPushListener() {
    homeCamera = TuyaHomeSdk.getCameraInstance();
    if (homeCamera != null) {
        homeCamera.registerCameraPushListener(mTuyaGetBeanCallback);
    }
}

/**
 * Logout listening after account logout
 */
public void unRegisterCameraPushListener() {
    if (homeCamera != null) {
        homeCamera.unRegisterCameraPushListener(mTuyaGetBeanCallback);
    }
}
```

> **Note**:
>
> 1. When the app process is killed, the listener is invalid. After the app logs in successfully, register for monitoring; the app logs out and cancels the monitoring
> 2. To avoid repeated push messages (user own channel pushed, generated by Tuya assist protocol), you can filter by message id, message type, and message arrival time



### Multi-language configuration in the panel

Download the  [language](https://github.com/TuyaInc/tuya_smart_bizbundle_language) , choose the configuration you need and copy it to the res
# IPC Device bizBundle

## Features

TuyaSmart Android IPC biz Bundle (referred to as: TuyaCameraPanelSDK ) is a series of panel SDKs related to camera functions developed based on [Tuya Smart Camera SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/ipc/). It mainly includes the following functions:

- Preview panel, playback panel, cloud storage panel, message center panel, album panel, settings panel.



###  Integrated SDK

1. [Access service package framework](./access.md)

2. Configure build.gradle in the module directory, add the dependencies:

```java
   dependencies {
      implementation 'com.tuya.smart:tuyasmart-bizbundle-camera:3.20.0-5'
   }
```

## Function

TuyaCameraPanelSDK is an external public interface of Tuya Smart Camera Panel SDK, which includes jump and custom implementation of multiple panels.

Please refer to the demo procedure to configure a successful camera and perform operations related to the camera panel.

> [TuyaSmartBizbundle Demo](https://registry.code.tuya-inc.top/android-developer/tuyasmart-bizbundle-demo.git)



### Native Preview Panel

Camera native preview panel, including real-time video preview, sharpness switch, sound switch control, screenshot, recording, intercom and other functions, motion detection, PTZ direction control, favorite point addition / deletion, cruise control, etc.

**Declaration**

The panel jumps through the route. The routing address is camera_ panel_2

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | device id   |

**Example**

```java
  Bundle bundle = new Bundle();
  bundle.putString("extra_camera_uuid", devId);
  UrlBuilder urlBuilder = new UrlBuilder(context, "camera_panel_2").putExtras(bundle);
  UrlRouter.execute(urlBuilder);
```

###  RN Preview Panel

Camera RN preview panel, packaged in TuyaPanelSDK, refer to [Device Control](./panel_device.md)

**Declaration**

AbsPanelCallerService.goPanelWithCheckAndTip() to jump to the RN panel

```java
 AbsPanelCallerService service = MicroContext.getServiceManager().findServiceByInterface(AbsPanelCallerService.class.getName());
 service.goPanelWithCheckAndTip(IPCPanelActivity.this, bean.getDevId());
```

### Playback Panel

The camera playback panel displays the video saved on the camera storage device, including video playback, playback date selection, video drag and play with time axis, play / pause, sound control, screenshot, recording and other functions

**Declaration**

The panel jumps through the route. The routing address is camera_playback_panel

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | Device id   |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putExtra("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context,"camera_playback_panel").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```

### Cloud Storage Panel

The camera's cloud storage panel displays recorded cloud videos recorded after the cloud storage function is enabled. Including video cloud storage playback, cloud storage date selection, video drag and play with time axis, play / pause, sound control, screenshot, recording and other functions, and motion detection data list display.

**Declaration**

The panel jumps through the route. The routing address is camera_cloud_panel

**Parameters**

| Parameter         | Description                                                  |
| :---------------- | :----------------------------------------------------------- |
| extra_camera_uuid | Device id                                                    |
| timeRangeBean     | Cloud storage of a piece of data's position from the last day ,not necessary |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putExtra("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context,"camera_cloud_panel").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```

###  Message-Center Panel

The camera message center panel includes all kinds of messages generated during the recording process of the camera. It is displayed by date and message type. The message category supports pictures, videos, and pure audio. It can be previewed and deleted.

**Declaration**

The panel jumps through the route. The routing address is camera_message_panel

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | device id   |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putExtra("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context,"camera_message_panel").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```

### Album Panel

The camera album panel shows the files saved according to the device id. These files are local screenshots and recorded videos during the camera preview / playback / cloud video playback process. Can perform preview, single delete, delete all operations.

**Declaration**

The panel jumps through the route. The routing address is camera_local_video_photo

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | Device id   |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putExtra("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context,"camera_local_video_photo").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```

### Doorbell Panel

The camera doorbell answer panel displays the doorbell message interface that is pushed over, including the basic information of the doorbell, real-time screenshot, answer, and hang up functions; it will enter the camera preview panel after the answer is successful

**Declaration**

The panel jumps through the route. The routing address is camera_door_bell

**Parameters**

| Parameter | Description                                                |
| :-------- | :--------------------------------------------------------- |
| devId     | device id，It is usually extracted from the message pushed |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putString("devId", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(MicroContext.getApplication(), "camera_door_bell").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```

### Video Doorbell Panel

The camera video stream doorbell answer panel displays the real-time video stream doorbell message interface that is pushed over, including doorbell status information, answering, and hang-up functions; the doorbell answering timeliness, dwell time < 25s, and real-time video call after successful answer.

**Declaration**

The panel jumps through the route. The routing address is camera_action_doorbell

**Parameters**

| Parameter           | Description                                                  |
| :------------------ | :----------------------------------------------------------- |
| extra_camera_uuid   | device id,it is usually extracted from the message pushed    |
| doorbell_start_time | Long type, which indicates the start time of pressing the doorbell. The doorbell answering panel counts from the time of pressing, and the stay time is 25s (actually < 25s) |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putString("extra_camera_uuid", deviceId);
bundle.putLong("doorbell_start_time", startTime);
UrlBuilder urlBuilder = new UrlBuilder(MicroContext.getApplication(),  "camera_action_doorbell").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```

### Set Panel

Camera settings panel, which can be configured and displayed through the background dp point（[Tuya Smart Camera SDK - Equipment function point](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/ipc/camera_device_points.html))，include：

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

**Declaration**

The panel jumps through the route. The routing address is camera_panel_more

**Parameters**

| Parameter         | Description |
| :---------------- | :---------- |
| extra_camera_uuid | device id   |

**Example**

```java
Bundle bundle = new Bundle();
bundle.putString("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context, panel).putExtras(bundle);
UrlRouter.execute(urlBuilder);
```

> Note: if it is found that the configuration route cannot enter the settings page, please contact the corresponding project manager to confirm whether it is the new settings panel



### Theme Style

Themes include two types: black and white, support the Playback Panel, Cloud Storage Panel, Settings Panel, Message-Center Panel and Local Album panel of Tuya Smart Camera

**Declaration**

Users can set the theme through CameraUIThemeUtils.setCurrentThemeId(@ThemeIDs int themeIDs) method

  ```java
  CameraUIThemeUtils.setCurrentThemeId(@ThemeIDs int themeIDs);
  ```

**Parameters**

| Parameter | Description                                          |
| :-------- | :--------------------------------------------------- |
| themeId   | ThemeIDs annotation：BLACK_THEME_ID； WHITE_THEME_ID |

 **Example**

  ```java
 CameraUIThemeUtils.setCurrentThemeId(Constants.BLACK_THEME_ID);
  ```

  > Called when the panel enters. The global influence is set. Preview panel is not supported

###  PanelSDK customization

TuyaSmartCameraPanelBizBundle supports user-defined configuration panel. If users implement the panel function by themselves, please refer to [Tuya Smart Camera SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/ipc/)

> The BizBundle has integrated with the SDK. When developing with reference to the SDK document, the version should be aligned with the BizBundle.

**Implementation**

Enter the assets folder of the app and find the service configuration file module_ app.json , find the route address to be intercepted for deletion, and capture the deleted route for custom jump.

**Example**

customize the implementation settings page

1. Find the set routing address "camera_ panel_ more" to delete.

   ![图片](../../images/tuya_smart_bizbundle_ipc_1.png)

2. Refer to [Integration](./access.html) , get the unfulfilled routing address and jump to the corresponding page

```java
    TuyaWrapper.init(this, new RouteEventListener() {
           @Override
           public void onFaild(int errorCode, UrlBuilder urlBuilder) {
               // urlBuilder.originUrl
               ToastUtil.shortToast(TuyaPanelSDK.getCurrentActivity(), urlBuilder.originUrl);
           }
       },new ServiceEventListener() {
            @Override
            public void onFaild(String serviceName) {
                Log.e("service not implement", serviceName);
            }
    });
```

### Routing table

| Routing target                 | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| camera_panel_2                 | Native Preview Panel                                         |
| camera_playback_panel          | Playback Panel                                               |
| camera_cloud_panel             | Cloud Storage Panel                                          |
| camera_message_panel           | Message-Center Panel                                         |
| camera_door_bell               | Doorbell Panel                                               |
| doorbell_camera_panel          | Doorbell Preview Panel                                       |
| doorbell_camera_playback_panel | Doorbell Playback Panel                                      |
| camera_action_doorbell         | Video Doorbell Panel                                         |
| camera_panel_more              | Set Panel                                                    |
| dev_base_info                  | Set - Modify Device Name                                     |
| camera_panel_info              | Set - DeviceInfo                                             |
| helpCenter                     | Set - Feedback [feedback bizBundle](./faq.md)                |
| dev_share_edit                 | Set - Device Sharing                                         |
| not_share_support_help         | Set - Device Not Support Sharing                             |
| AbsCameraOTAService            | Set  - Firmware Information, you need to implement the checkupgradefirmware method of this service |

> Some functions on the settings panel need to be implemented by yourself.

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
   
      mModifyDevInfoInteractor.modifyDeviceImg(deviceId, panelName, new File(iconFilePath),
              new ModifyDevInfoInteractor.ModifyDeviceImgCallback() {
                  @Override
                  public void onModifyDeviceImgSuccess(String url) {
  
                    
                  }
  
                  @Override
                  public void onModifyDeviceImgFailure() {
  
             
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

[language](./access.md) 
# 摄像机业务包


 ## 功能介绍

涂鸦智能 Android IPC 业务包（ TuyaSmartCameraPanelBizBundle ）是基于 [Tuya Smart Camera SDK](<https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/ipc/>) 开发的一系列摄像机功能相关的面板 SDK。主要包括以下功能：

- 预览面板，回放面板，云存储面板，消息中心面板，相册面板，设置面板。



 ## 业务包集成

1. [接入业务包框架](./access.md)

3. 模块 build.gradle 配置 dependencies 依赖

```java
   dependencies {

      implementation 'com.tuya.smart:tuyasmart-bizbundle-camera:3.20.0-5'
   
      //... 其他配置
   }
```




## 功能调用

TuyaSmartCameraPanelBizBundle 是涂鸦智能摄像机各面板的调用对外暴露的接口，包含多个面板的跳转及自定义实现。

参考 demo下 ipc 部分流程，配置成功摄像头，进行摄像头面板相关操作

> [TuyaSmartBizbundle Demo](https://registry.code.tuya-inc.top/android-developer/tuyasmart-bizbundle-demo.git)



### Native预览面板

摄像机原生预览面板，包括视频实时预览，清晰度切换，声音开关控制，截图，录制，对讲等功能，移动侦测，PTZ方向控制，收藏点添加/删除，巡航控制等

**接口说明**

面板通过路由进行跳转，路由地址：camera_panel_2

**参数说明**

| 参数              | 说明                                      |
| :---------------- | :---------------------------------------- |
| extra_camera_uuid | 设备 id，通过设备家庭下面的设备列表下获取 |

**示例代码**

```java
  Bundle bundle = new Bundle();
  bundle.putString("extra_camera_uuid", devId);
  UrlBuilder urlBuilder = new UrlBuilder(context, "camera_panel_2").putExtras(bundle);
  UrlRouter.execute(urlBuilder);
```



###  RN预览面板

摄像机 RN 预览面板，需要事先集成设备控制业务包，参考 [设备面板](./panel_device)

**接口说明**

通过服务化接口 AbsPanelCallerService.goPanelWithCheckAndTip() 可跳转到 RN 面板

```java
 AbsPanelCallerService service = MicroContext.getServiceManager().findServiceByInterface(AbsPanelCallerService.class.getName());
 service.goPanelWithCheckAndTip(IPCPanelActivity.this, bean.getDevId());
```



### 回放面板

摄像机回放面板，展示的是保存在摄像机存储设备上的视频，包括视频回放，回放日期选择，视频随时间轴拖动播放，播放/暂停，声音控制，截图，录制等功能

**接口说明**

面板通过路由进行跳转，路由地址：camera_playback_panel

**参数说明**

| 参数              | 说明    |
| :---------------- | :------ |
| extra_camera_uuid | 设备 id |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putExtra("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context,"camera_playback_panel").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```



### 云存储面板

摄像机云存储面板，展示开通云存储功能之后录制保存的云端视频。包括视频云存储播放，云存储日期选择，视频随时间轴拖动播放，播放/暂停，声音控制，截图，录制等功能，移动侦测数据列表展示。

**接口说明**

面板通过路由进行跳转，路由地址：camera_cloud_panel

**参数说明**

| 参数              | 说明                                      |
| :---------------- | :---------------------------------------- |
| extra_camera_uuid | 设备 id                                   |
| timeRangeBean     | 云存储最近一天的某条数据 position，可不传 |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putExtra("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context,"camera_cloud_panel").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```



###  消息中心面板

摄像机消息中心面板，包括摄像机录制过程中产生的各类消息，按日期，消息类型进行展示，消息类别支持图片，视频，纯音频等，可进行预览及单条删除，全部删除操作。

**接口说明**

面板通过路由进行跳转，路由地址：camera_message_panel

**参数说明**

| 参数              | 说明    |
| :---------------- | :------ |
| extra_camera_uuid | 设备 id |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putExtra("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context,"camera_message_panel").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```



### 相册面板

摄像机相册面板，展示的是根据设备 id 所保存文件，这些文件是在摄像机预览/回放/云视频播放过程中，本地截图和录制视频。可进行预览，单条删除，全部删除等操作。

**接口说明**

面板通过路由进行跳转，路由地址：camera_local_video_photo

**参数说明**

| 参数              | 说明    |
| :---------------- | :------ |
| extra_camera_uuid | 设备 id |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putExtra("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context,"camera_local_video_photo").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```



### 门铃面板

摄像机门铃呼叫接听面板，显示推送过来的门铃消息界面，包括门铃基本信息，实时截图，接听/挂断功能；接听成功后会进入摄像机预览面板

**接口说明**

面板通过路由进行跳转，路由地址：camera_door_bell

**参数说明**

| 参数  | 说明                                  |
| :---- | :------------------------------------ |
| devId | 设备 id，一般通过推送过来的消息中提取 |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putString("devId", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(MicroContext.getApplication(), "camera_door_bell").putExtras(bundle);
UrlRouter.execute(urlBuilder);
```



### 视频流门铃面板

摄像机视频流门铃接听面板，显示推送过来的实时视频流门铃消息界面，包括门铃状态信息，接听，挂断功能；门铃接听有时效性，停留时间 < 25s，接听成功后进行实时视频通话。

**接口说明**

面板通过路由进行跳转，路由地址：camera_action_doorbell

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
UrlBuilder urlBuilder = new UrlBuilder(MicroContext.getApplication(),  "camera_action_doorbell").putExtras(bundle);
UrlRouter.execute(urlBuilder);
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

**接口说明**

面板通过路由进行跳转，路由地址：camera_panel_more

**参数说明**

| 参数              | 说明    |
| :---------------- | :------ |
| extra_camera_uuid | 设备 id |

**示例代码**

```java
Bundle bundle = new Bundle();
bundle.putString("extra_camera_uuid", deviceId);
UrlBuilder urlBuilder = new UrlBuilder(context, panel).putExtras(bundle);
UrlRouter.execute(urlBuilder);
```

> 注意：如果发现配置路由进不了设置页面，请联系对应的项目经理，确认下是否是新设置面板



### 主题设置

主题包括黑色和白色2种，支持涂鸦智能摄像头的回放，云存储，消息中心，设置和本地相册

**接口说明**

用户可通过 CameraUIThemeUtils.setCurrentThemeId(@ThemeIDs int themeIDs) 方法来设置主题

```java
CameraUIThemeUtils.setCurrentThemeId(@ThemeIDs int themeIDs);
```

**参数说明**

| 参数    | 说明                                                         |
| :------ | :----------------------------------------------------------- |
| themeId | ThemeIDs 注解：BLACK_THEME_ID（黑色主题）； WHITE_THEME_ID（白色主题） |

**示例代码**

```java
CameraUIThemeUtils.setCurrentThemeId(Constants.BLACK_THEME_ID);
```

> 面板进入时调用，设置了影响全局；不支持预览面板



###  面板自定义

TuyaSmartCameraPanelBizBundle 支持用户自定义配置面板，用户若自行实现面板功能，可参考 [TuyaSmart Camera SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/ipc/) 。

> 业务包已集成 SDK，参考 SDK 文档开发时，版本应与业务包对齐。 

**实现方式**

进入 app 的 assets 文件夹下面，找到服务化配置文件 module_app.json，找到要拦截的路由地址进行删除，捕获已删除路由进行自定义跳转。

例：要自定义实现设置页面

1. 找到设置的路由地址 “camera_panel_more”，进行删除。

![图片](../../images/tuya_smart_bizbundle_ipc_1.png)

2. 参考 [框架接入- Application](./access.md#Application 初始化)，获取未实现路由地址跳转对应页面

     ```java
      TuyaWrapper.init(this, new RouteEventListener() {
             @Override
             public void onFaild(int errorCode, UrlBuilder urlBuilder) {
                 // 路由原始地址 urlBuilder.originUrl
                 ToastUtil.shortToast(TuyaPanelSDK.getCurrentActivity(), urlBuilder.originUrl);
             }
         },new ServiceEventListener() {
                 @Override
                 public void onFaild(String serviceName) {
                     Log.e("service not implement", serviceName);
                 }
         });
     ```



### 面板路由表

| 路由 target                    | 功能                |
| ------------------------------ | ------------------- |
| camera_panel_2                 | 黑色预览面板        |
| camera_playback_panel          | 回放面板            |
| camera_cloud_panel             | 云存储面板          |
| camera_message_panel           | 消息中心面板        |
| camera_door_bell               | 门铃来电接听面板    |
| doorbell_camera_panel          | 门铃预览面板        |
| doorbell_camera_playback_panel | 门铃回放面板        |
| camera_action_doorbell         | 直供电门铃接听面板  |
| camera_panel_more              | 设置面板            |
| dev_base_info                  | 设置 - 修改设备名称 |
| camera_panel_info              | 设置 - 设备信息功能 |
| helpCenter                     | 设置 - 意见反馈，参考 [反馈业务包](./faq.md) |
| dev_share_edit                 | 设置 - 设备分享           |
| not_share_support_help         | 设置 -设备不支持分享      |
| AbsCameraOTAService						| 设置 - 固件信息，需要自行实现此 service 的 checkUpgradeFirmware方法 |

> 设置面板上有些功能未实现，需要自行去实现。

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

[多语言配置](./access.md#res-配置)


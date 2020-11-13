# 云存储服务业务包

涂鸦智能摄像机提供云存储视频服务，通过此业务包可以开通云存储服务。开同云存储服务后，可以通过 [Tuya Smart Camera SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/ipc/cloud_video.html#%E4%BA%91%E5%AD%98%E5%82%A8) 查看和播放云存储视频。

## 集成业务包

1. [接入业务包框架](./access.md)
2. 云存储服务与账户强关联，需要集成涂鸦云用户管理，参考 [集成文档](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/User.html)
3. 在 module 的 build.gradle 配置


```java
dependencies {
  api 'com.tuya.smart:tuyasmart-bizbundle-cloud_storage:3.20.0-5'
}
```

## 功能调用

### 跳转到云存储购买页面

**接口说明**

跳转到云存储购买H5页面

> 云存储服务与账户强关联，因此需要在用户登录状态下才能正常调用

```java
public void buyCloudStorage(Context mContext, DeviceBean deviceBean, String homeId, AbsCloudCallback callback);
```

**参数说明**

| 参数       | 说明      |
| ---------- | --------- |
| context    | 上下文    |
| deviceBean | 设备x信息 |
| homeId     | 家庭 id   |
| callback   | 错误回调  |

**示例代码**

```java
findViewById(R.id.buy_btn).setOnClickListener(new View.OnClickListener() {
  @Override
  public void onClick(View v) {
    //获取服务
    AbsCameraCloudPurchaseService cameraCloudService = MicroServiceManager.getInstance().findServiceByInterface(AbsCameraCloudPurchaseService.class.getName());
    if (cameraCloudService != null) {
      cameraCloudService.buyCloudStorage(CameraCloudStorageActivity.this,
                                         TuyaHomeSdk.getDataInstance().getDeviceBean(devId),
                                         String.valueOf(FamilyManager.getInstance().getCurrentHomeId()), new AbsCloudCallback() {
                                           @Override
                                           public void onError(String errorCode, String errorMessage) {
                                             super.onError(errorCode, errorMessage);
                                           }
                                         });
    }
  }
});
```

### 释放资源

**接口说明**

跳转云存储购买页面中涉及网络请求等耗时操作，因此需要在调用的页面销毁时释放资源。

```java
public void destroy();
```

**示例代码**

```java
@Override
protected void onDestroy() {
  super.onDestroy();
  AbsCameraCloudPurchaseService cameraCloudService = MicroServiceManager.getInstance().findServiceByInterface(AbsCameraCloudPurchaseService.class.getName());
  if (cameraCloudService != null) {
  	cameraCloudService.destroy();
  }
}
```

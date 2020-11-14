# Cloud Service BizBundle

Tuya smart cameras provide cloud storage video services, and cloud storage services can be activated through this bizBundle. After active cloud storage service, you can view and play cloud storage videos through [Tuya Smart Camera SDK](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/ipc/cloud_video.html).

## Integrated BizBundle

1.  [Access service package framework](./access.md)
2.  Cloud storage service is strongly related to account, and needs to integrate graffiti cloud user management. Refer to [integrated document](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/User.html)
3.  Configure build.gradle in the module directory

```java
dependencies {
		implementation 'com.tuya.smart:tuyasmart-bizbundle-cloud_storage:3.20.0-5'
}
```

## Function

### Start cloud service purchase activity

**Declaration**

Jump to the cloud storage purchase H5 page

> Cloud storage services are strongly associated with user systems. Therefore, it needs to be called after use login.

```
public void buyCloudStorage(Context mContext, DeviceBean deviceBean, String homeId, AbsCloudCallback callback);
```

**Parameters**

| Parameter  | Description                                              |
| ---------- | -------------------------------------------------------- |
| context    | context                                                  |
| deviceBean | device info                                              |
| homeId     | The family ID is obtained through the home SDK interface |
| callback   | callback when error                                      |

**Example**

```java
findViewById(R.id.buy_btn).setOnClickListener(new View.OnClickListener() {
  @Override
  public void onClick(View v) {
    //get service
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

### Release resource

**Example**

Jumping to the cloud storage purchase page involves time-consuming operations such as network requests, so resources need to be released when the called page is destroyed.

```
public void destroy();
```

**Example**

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

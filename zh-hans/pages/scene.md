# 场景业务包

## 功能介绍

业务功能包括涂鸦智能场景模块的「添加智能」和「编辑智能」的业务逻辑和UI界面。

智能场景分为「一键执行场景」和「自动化场景」，下文分别简称为「场景」和「自动化」。

场景是用户添加动作，手动触发；自动化是由用户设定条件，当条件触发后自动执行设定的动作。

涂鸦云支持用户根据实际生活场景，通过设置气象或设备条件，当条件满足时，让一个或多个设备执行相应的任务。


## 业务包集成

### 创建工程

   在 Android Studio 中建立你的工程,接入公版 SDK 并配置完成

### 根目录的 build.gradle 配置

  ``` groovy
  allprojects {
      repositories {
        maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
        maven { url 'https://jitpack.io' }
      }
  }
  ```

### module 的 build.gradle 配置

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
      }
  
    lintOptions {
          abortOnError false
      }
  }
	dependencies {
	    implementation fileTree(dir: 'libs', include: ['*.jar'])
	    //scene require start
	    implementation 'com.android.support:appcompat-v7:28.0.0'
	    implementation "com.android.support:recyclerview-v7:28.0.0"
	    implementation "com.android.support:cardview-v7:28.0.0"
	    implementation "com.android.support:design:28.0.0"
	    implementation 'com.tuya.smart:tuyasmart:3.17.6-beta2'
	    implementation 'com.tuya.smart:tuyasmart-appshell:3.10.0'
	    implementation "com.tuya.smart:tuyasmart-base:3.17.0r139-rc.3"
	    implementation 'com.tuya.smart:tuyasmart-stencilwrapper:3.17.0.2r139'
	    implementation 'com.tuya.smart:tuyasmart-stencilmodel:3.17.0r139-rc.2'
	    implementation "com.tuya.smart:tuyasmart-framework:3.17.0.2r139-external"
	    implementation 'com.tuya.smart:tuyasmart-uispecs:0.0.9'
	    implementation 'jp.wasabeef:recyclerview-animators:2.2.4'
	    implementation 'com.alibaba:fastjson:1.1.67.android'
	    implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
	    implementation 'io.reactivex.rxjava2:rxandroid:2.1.1'
	    implementation 'io.reactivex.rxjava2:rxjava:2.2.9'
	    implementation 'com.facebook.fresco:fresco:1.13.0'
	    implementation 'com.facebook.fresco:imagepipeline-okhttp3:1.3.0'
	    implementation 'com.tuya.smart:tuyasmart-uiadapter:3.13.3r129-rc.4'
	    implementation 'com.google.android:flexbox:0.2.5'
	    implementation 'com.facebook.fbui.textlayoutbuilder:textlayoutbuilder:1.4.0'
	    implementation 'com.facebook.react:react-native:0.51.1.11'
	    implementation 'com.tuya.smart:tuyasmart-tuya-mist-litho-base:3.13.0r127-rc.7'
	    implementation 'com.tuya.android:mist-litho:1.3.30'
	    implementation 'com.tuya.android:mist-litho-fresco:1.3.30'
	    implementation 'com.tuya.android:mist-litho-sections-widget:1.3.30'
	    implementation 'com.tuya.android:mist-litho-widget:1.3.30'
	    implementation 'com.tuya.android:mist-litho-sections-core:1.3.30'
	    implementation 'com.tuya.android:mist-litho-annotation:1.0.0'
	    implementation 'com.tuya.android:mist-litho-core:1.3.30'
	    implementation 'com.tuya.smart:tuyasmart-scene:3.17.6r141-rc.7'
	    implementation 'com.tuya.smart:tuyasmart-scene-business-api:3.17.6r141-rc.1'
	    //scene require end  	
    }
  ```

## 服务协议

### 提供服务

场景业务包实现 `ITuyaSceneBusinessService`以提供服务。

**示例代码**

``` java
  //获取场景业务包服务
ITuyaSceneBusinessService iTuyaSceneBusinessService = MicroContext.findServiceByInterface(ITuyaSceneBusinessService.class.getName());
```
### 创建场景

通过获取家庭 Id 进入场景添加页 

**接口说明**

进入创建场景页

``` java
ITuyaSceneBusinessService.addScene(Activity activity, long homeId, int requestCode);;
```
**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| activity     | Activity 对象 |
| homeId       | 家庭 Id 通过公版 SDK 接口获取                                                             |
| requestCode | 请求 code，在 onActivityResult 的时候带回                                        |

**示例代码**

``` java
if(null != iTuyaSceneBusinessService && HOME_ID != 0){
            iTuyaSceneBusinessService.addScene(activity, homeId, requestCode);
        }      
```

### 编辑场景
通过获取场景数据和家庭 Id进入场景编辑页面

**接口说明**

进入编辑场景页

``` java
ITuyaSceneBusinessService.editScene(Activity activity, long homeId,SceneBean sceneBean, int requestCode);;
```

**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| activity     | Activity 对象 |
| homeId       | 家庭 Id 通过公版 SDK 接口获取    
| SceneBean       | 场景数据对象，通过 SDK 获取场景列表接口获取                                                              |
| requestCode | 请求 code，在 onActivityResult 的时候带回                                        |


**示例代码**

``` java
TuyaHomeSdk.getSceneManagerInstance().getSceneList(homeId, new ITuyaResultCallback<List<SceneBean>>() {
            @Override
            public void onSuccess(List<SceneBean> result) {
                if(!result.isEmpty()){
                    SceneBean sceneBean = result.get(0);
                    if(null != iTuyaSceneBusinessService){
                        iTuyaSceneBusinessService.editScene(SceneActivity.this, homeId, sceneBean, requestCode);
                    }
                }
            }

            @Override
            public void onError(String errorCode, String errorMessage) {

            }
        });
    
```

### 设置地理位置
场景的条件和生效时间段需要设置app的地理位置，如果不设置，将不会在选择条件时自动获取城市信息，但是还是可以在城市列表自行选择

**接口说明**

设置场景条件地理位置

``` java
ITuyaSceneBusinessService.setAppLocatione(double longitude, double latitude);
```

**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| longitude     | 经度 业务app自行接入的相关三方地图提供|
| latitude       | 纬度 业务app自行接入的相关三方地图提供


**示例代码**

``` java

if(null != iTuyaSceneBusinessService){           
	iTuyaSceneBusinessService.setAppLocation(lng, lat);
}    
```

### 设置App地图类
场景条件中的地理位置，如果不需要国外账号就无需调用，在国外账号需要设置地图类，不设置默认走获取国内城市接口

**接口说明**

设置地图类对象

``` java
ITuyaSceneBusinessService.setMapActivity(Class activity);
```

**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| Class     | 地图Activity 类对象 |



**示例代码**

``` java
if(null != iTuyaSceneBusinessService){
            //TODO 业务方地图Activity
            iTuyaSceneBusinessService.setMapActivity(MapActivity.class);
        }    
```
### 保存地图选点数据
接入地图类之后，需要将地图选点数据发给业务包，用以更新条件地理位置信息

**接口说明**

设置地图类对象

``` java
ITuyaSceneBusinessService.saveMapData(double longitude, double latitude,String city, String address);
```

**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| longitude     | 经度 |
| latitude       | 纬度    
| city       | 城市信息|
| address | 地址信息                                        |

**示例代码**

``` java
if(null != iTuyaSceneBusinessService){    
   iTuyaSceneBusinessService.saveMapData(lng, lat, city, address);
 }   
```
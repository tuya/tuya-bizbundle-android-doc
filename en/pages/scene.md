# Scene Biz Bundle

## Features

The business functions include the business logic and UI interface of "add scene" and "edit scene" of the Tuya smart scene module.

Intelligent scenes are divided into "one-key execution scenes" and "automated scenes", hereinafter referred to as "scene" and "automation", respectively.

The scenario is that the user adds an action and triggers it manually; the automation is a condition set by the user, and when the condition is triggered, the set action is automatically executed.

Tuya Cloud supports users to set weather or equipment conditions based on actual life scenarios. When the conditions are met, let one or more equipment perform the corresponding tasks.


## Biz Bundle integration

### Create project

   Create your project in Android Studio, connect to the public SDK and configure it

### The build.gradle configuration of the root directory

  ``` groovy
  allprojects {
      repositories {
        maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
        maven { url 'https://jitpack.io' }
      }
  }
  ```

### module's build.gradle configuration

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
	    implementation 'com.tuya.android:mist-litho:1.3.31'
	    implementation 'com.tuya.android:mist-litho-fresco:1.3.30'
	    implementation 'com.tuya.android:mist-litho-sections-widget:1.3.30'
	    implementation 'com.tuya.android:mist-litho-widget:1.3.30'
	    implementation 'com.tuya.android:mist-litho-sections-core:1.3.30'
	    implementation 'com.tuya.android:mist-litho-annotation:1.0.0'
	    implementation 'com.tuya.android:mist-litho-core:1.3.30'
	    implementation 'com.tuya.smart:tuyasmart-scene:3.17.6r141-rc.15'
	    implementation 'com.tuya.smart:tuyasmart-scene-business-api:3.17.6r141-rc.1'
	    implementation 'com.readystatesoftware.systembartint:systembartint:1.0.3'  
	    //scene require end  	
    }
  ```

### Initialize operation in Application

 ``` java
 
   SoLoader.init(this, false);
   MistConfig config = new MistConfig();
   config.create();
   MistCore.getInstance().init(config, this);
 
 ``` 

## Service Agreement

### Provide services

The scene biz bundle implements `ITuyaSceneBusinessService` to provide services.

**Example**

``` java
 //Get scene bizbundle service
ITuyaSceneBusinessService iTuyaSceneBusinessService = MicroContext.findServiceByInterface(ITuyaSceneBusinessService.class.getName());
```
### Create Scene

Enter the scene adding page by getting the family ID

**Declaration**

Go to create scene page

``` java
ITuyaSceneBusinessService.addScene(Activity activity, long homeId, int requestCode);;
```
**Parameters**

| Parameter         | Description                                                         |
| ------------ | ------------------------------------------------------------ |
| activity     | Activity Object |
| homeId       | The family ID is obtained through the home SDK interface                                                            |
| requestCode | Request code, bring back in onActivityResult                                     |

**Example**

``` java
if(null != iTuyaSceneBusinessService && HOME_ID != 0){
            iTuyaSceneBusinessService.addScene(activity, homeId, requestCode);
        }      
```

### Edit Scene
Enter scene edit page by obtaining scene data and family ID

**Declaration**

Enter edit scene

``` java
ITuyaSceneBusinessService.editScene(Activity activity, long homeId,SceneBean sceneBean, int requestCode);;
```

**Parameters**

| Parameter         | Description                                                         |
| ------------ | ------------------------------------------------------------ |
| activity     | Activity Object |
| homeId       | The family ID is obtained through the home SDK interface 
| SceneBean       | Scene data object, obtained through SDK Get Scene List interface                                                             |
| requestCode | Request code, bring back in onActivityResult                                        |


**Example**

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

### Set geographic location

The conditions of the scene and the effective time period need to set the geographic location of the app. If it is not set, the city information will not be automatically obtained when the condition is selected, but it can still be selected on the city list.

**Declaration**

Set scene conditions‘s location

``` java
ITuyaSceneBusinessService.setAppLocatione(double longitude, double latitude);
```

**Parameters**

| Parameter         | Description                                                          |
| ------------ | ------------------------------------------------------------ |
| longitude     | Longitude,Long-distance business app provides access to related tripartite maps|
| latitude       | Latitude,Provide relevant three-party maps for self-service access


**Example**

``` java

if(null != iTuyaSceneBusinessService){           
	iTuyaSceneBusinessService.setAppLocation(lng, lat);
}    
```

### Set App Map Class

The geographical location in the scene conditions, if you do not need a foreign account, you do not need to call, you need to set the map category in the foreign account, do not set the default to get the domestic city interface

**Declaration**

Set App Map Class

``` java
ITuyaSceneBusinessService.setMapActivity(Class activity);
```

**Parameters**

| Parameter         | Description                                                           |
| ------------ | ------------------------------------------------------------ |
| Class     |Map Activity class object |



**Example**

``` java
if(null != iTuyaSceneBusinessService){
          
            iTuyaSceneBusinessService.setMapActivity(MapActivity.class);
        }    
```
### Save map selection data

After accessing the map class, you need to send the map selection data to the service package to update the conditional geographic location information

**Declaration**

Set up map data

``` java
ITuyaSceneBusinessService.saveMapData(double longitude, double latitude,String city, String address);
```

**Parameters**

| Parameter         | Description                                                          |
| ------------ | ------------------------------------------------------------ |
| longitude     | longitude |
| latitude       | latitude    
| city       | city |
| address | address                                      |

**Example**

``` java
if(null != iTuyaSceneBusinessService){    
   iTuyaSceneBusinessService.saveMapData(lng, lat, city, address);
 }   
```
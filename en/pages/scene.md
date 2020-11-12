# Scene Biz Bundle

## Features

The business functions include the business logic and UI interface of "add scene" and "edit scene" of the Tuya smart scene module.

Intelligent scenes are divided into "one-key execution scenes" and "automated scenes", hereinafter referred to as "scene" and "automation", respectively.

The scenario is that the user adds an action and triggers it manually; the automation is a condition set by the user, and when the condition is triggered, the set action is automatically executed.

Tuya Cloud supports users to set weather or equipment conditions based on actual life scenarios. When the conditions are met, let one or more equipment perform the corresponding tasks.


## Biz Bundle integration

### Create project

   Create your project in Android Studio, connect to the public SDK and configure [Biz Bundle Framework](./access.md)

### module's build.gradle configuration

``` groovy
dependencies {
  implementation 'com.tuya.smart:tuyasmart-bizbundle-scene:3.20.0-5'
}
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

| Parameter | Description |
| --- | --- |
| activity | Activity Object |
| homeId | The family ID is obtained through the home SDK interface |
| requestCode | Request code, bring back in onActivityResult |

**Example**

``` java
if(null != iTuyaSceneBusinessService && homeId != 0){
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

| Parameter | Description |
| --- | --- |
| activity | Activity Object |
| homeId | The family ID is obtained through the home SDK interface 
| SceneBean | Scene data object, obtained through SDK Get Scene List interface |
| requestCode | Request code, bring back in onActivityResult |


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
ITuyaSceneBusinessService.setAppLocation(double longitude, double latitude);
```

**Parameters**

| Parameter | Description |
| --- | --- |
| longitude | Longitude,Long-distance business app provides access to related tripartite maps |
| latitude | Latitude,Provide relevant three-party maps for self-service access |


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

| Parameter | Description |
| --- | --- |
| Class | Map Activity class object |



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

| Parameter | Description |
| --- | --- |
| longitude | longitude |
| latitude | latitude |
| city  city |
| address | address |

**Example**

``` java
if(null != iTuyaSceneBusinessService){    
   iTuyaSceneBusinessService.saveMapData(lng, lat, city, address);
 }
```
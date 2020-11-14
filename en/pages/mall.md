# H5 Mall

## Features Overview

H5 Mall provides an container that hosts the "App mall", so that your app has powerful mall capabilities and allows mobile traffic to be realized through the mall.
> "App mall" is a value-added service provided by Tuya platform. For details, you can serach "App mall" in Tuya Smart Platform [Value-added Service](https://www.tuya.com/vas/)

## Integrated Mall Biz Bundle

### Create Project

Build your project in the Android Studio and integrate Tuyasmart HomeSDK and completion of the bizbundle[access](./access.md)

### Configure the module build.gradle

  ``` groovy
	dependencies {
    	dependencies {
        api 'com.tuya.smart:tuyasmart-bizbundle-mall:3.20.0-5'
  	}
  ```

### Configure the progurad

  ``` 
  # Configure all third dependencies progurad rules

  #fastJson
  -keep class com.alibaba.fastjson.**{*;}
  -dontwarn com.alibaba.fastjson.**    
    
  #mqtt
  -keep class com.tuya.smart.mqttclient.mqttv3.** { *; }
  -dontwarn com.tuya.smart.mqttclient.mqttv3.** 

  -keep class com.squareup.okhttp.** { *; }
  -keep interface com.squareup.okhttp.** { *; }
  -dontwarn com.squareup.okhttp.**    

  -keep class okio.** { *; }
  -dontwarn okio.**    
  
  -keep class com.tuya.**{*;}
  -dontwarn com.tuya.**
  ```

## Mall Function
  
### Mall service availability

Availability of mall service in the current user's area

**Declaration**

Availability of mall service in the current user's area

``` java
isSupportMall()
```
**sample code**
``` java
    TuyaMallService service = MicroContext.getServiceManager().findServiceByInterface(TuyaMallService.class.getName());
    boolean mallEnable = service.isSupportMall()
```

### Mall home url

If the mall service is available, you can get the URL of the home page of the mall in user's area

**Declaration**

Get the URL of the home page of the mall in user's area,This method is an asynchronous method.

``` java
requestMallHome(IGetMallUrlCallback callback)
```
**Parameter**

| Parameter                          | Description                            |
| ----------------------------- | ------------------------------- |
| IGetMallUrlCallback | mall home url callbacks |

**Example**
``` java
    TuyaMallService service = MicroContext.getServiceManager().findServiceByInterface(TuyaMallService.class.getName());
    service.requestMallHome(new IGetMallUrlCallback() {
        @Override
        public void onSuccess(String url) {
            Log.i("mall url = ",url);
        }
        @Override
        public void onError(String code, String error) {
        }
    });
```

### Mall order url

If the mall service is available, you can get the URL of the order of the mall in user's area

**Declaration**

get the URL of the order of the mall in user's area,This method is an asynchronous method.

``` java
requestMallUserCenter(IGetMallUrlCallback callback)
```
**Parameter**

| Parameter                 | Description                     |
| -------------------- | ------------------------ |
| IGetMallUrlCallback | mall order url callbacks |

**Example**
``` java
    TuyaMallService service = MicroContext.getServiceManager().findServiceByInterface(TuyaMallService.class.getName());
    service.requestMallUserCenter(new IGetMallUrlCallback() {
        @Override
        public void onSuccess(String url) {
            Log.i("mall user center url = ", url);
        }
        @Override
        public void onError(String code, String error) {
        }
    });
```

### Open mall page

Activity and Fragment support on the Mall display page

**Example**

Activity
``` java
Intent intent = new Intent(context, WebViewActivity.class);
intent.putExtra("Uri", url);
context.startActivity(intent);
```

Fragment
``` java
WebViewFragment fragment = new WebViewFragment();
Bundle args = new Bundle();
args.putString("Uri", url);
args.putBoolean("enableLeftArea", true);
fragment.setArguments(args);
getSupportFragmentManager().beginTransaction()
        .add(R.id.web_content, fragment, WebViewFragment.class.getSimpleName())
        .commit();
```
# H5 Mall

## Features Overview

H5 Mall provides an container that hosts the "App mall", so that your app has powerful mall capabilities and allows mobile traffic to be realized through the mall.
> "App mall" is a value-added service provided by Tuya platform. For details, you can serach "App mall" in Tuya Smart Platform [Value-added Service](https://www.tuya.com/vas/)

## Integrated Mall Biz Bundle

### Create Project

Build your project in the Android Studio and integrate Tuyasmart HomeSDK

### Configure the root build.gradle

  ``` groovy
  buildscript {
      repositories {
          maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
      }
      dependencies {
          classpath 'com.tuya.android.module:tymodule-config:0.4.0-SNAPSHOT'
      }
  }

  allprojects {
      repositories {
          maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
      }
  }
  ```

### Configure the module build.gradle

  ``` groovy
	dependencies {
    //require start
    implementation 'com.tuya.smart:tuyasmart:3.15.0-beta3'
    implementation 'com.tuya.smart:tuyasmart-webcontainer:3.17.6r141-open'
    implementation 'com.tuya.smart:tuyasmart-tuyamall-sdk:1.0.2'
    implementation 'com.tuya.smart:optimus:1.0.0'
    annotationProcessor 'com.tuya.smart:optimus-compiler:1.0.0'
    implementation 'com.tuya.smart:tuyasmart-xplatformmanager:1.0.0'
    implementation 'com.tuya.smart:tuyasmart-appshell:3.10.0'
    implementation "com.tuya.smart:tuyasmart-base:3.17.0r139-rc.3"
    implementation 'com.tuya.smart:tuyasmart-stencilwrapper:3.17.0.1r139'
    implementation "com.tuya.smart:tuyasmart-framework:3.17.0.2r139-external"
    implementation 'com.tuya.smart:tuyasmart-uispecs:0.0.5'
    implementation "com.tuya.smart:tuyasmart-picture:3.12.0r123"
    implementation "com.tuya.smart:tuyasmart-rpc:3.12.0r123"
    implementation "com.tuya.smart:tuyasmart-video:3.12.6r125"
    implementation "com.tuya.smart:tuyasmart-ipc-videoview:3.13.0r125-open-SNAPSHOT"
    //require end

    implementation 'com.alibaba:fastjson:1.1.67.android'
    implementation 'org.eclipse.paho:org.eclipse.paho.client.mqttv3:1.2.0'
    implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
    implementation 'io.reactivex.rxjava2:rxandroid:2.1.1'
    implementation 'io.reactivex.rxjava2:rxjava:2.2.9'
    implementation 'com.facebook.fresco:fresco:1.13.0'
    implementation "com.facebook.fresco:imagepipeline-okhttp3:1.3.0"
  	}
  ```

### Configure the styles

  ``` xml 
  <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
      <item name="app_bg_color">#FFFFFF</item>
  </style>
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

### Init Tuya Smart Mall Biz Bundle in the Application

  ``` java
    public class TuyaSmartApp extends Application {

        @Override
        public void onCreate() {
            super.onCreate();
            Fresco.initialize(this);
            TuyaWrapper.init(this);
            TuyaHomeSdk.init(this);
            TuyaOptimusSdk.init(this);
        }
    }
  ```
  
## Mall Function
  
### Mall service availability

Availability of mall service in the current user's area

**Declaration**

Availability of mall service in the current user's area,This method is an asynchronous method.

``` java
requestSupportMall(ResultListener<Boolean> listener)
```
**Parameter**

| Parameter         | Description                                                         |
| ------------ | ------------------------------------------------------------ |
| listener     |  mall service  availability callbacks |

**Example**
``` java
ITuyaMallSdk iTuyaMallSdk = TuyaOptimusSdk.getManager(ITuyaMallSdk.class);
iTuyaMallSdk.requestSupportMall(new Business.ResultListener<Boolean>() {
    @Override
    public void onFailure(BusinessResponse businessResponse, Boolean aBoolean, String s) {
            L.e("requestSupportMall", s);
    }
    @Override
    public void onSuccess(BusinessResponse businessResponse, Boolean aBoolean, String s) {
            //aBoolean = true mall service is availabe
            L.d("requestSupportMall", String.valueOf(aBoolean));
    }
});
```

### Mall home url

If the mall service is available, you can get the URL of the home page of the mall in user's area

**Declaration**

Get the URL of the home page of the mall in user's area,This method is an asynchronous method.

``` java
requestHomePageUrl(IQueryMallPageUrlCallback callback)
    ```
**Parameter**

| Parameter                          | Description                            |
| ----------------------------- | ------------------------------- |
| IQueryMallPageUrlCallback | mall home url callbacks |

**Example**
``` java
ITuyaMallSdk iTuyaMallSdk = TuyaOptimusSdk.getManager(ITuyaMallSdk.class);
iTuyaMallSdk.requestHomePageUrl(new IQueryMallPageUrlCallback() {
    @Override
    public void onSuccess(String domain) {
        L.d("requestHomePageUrl", domain);
    }
    @Override
    public void onError(String code, String error) {
        L.e("requestHomePageUrl", error);
    }
});
```


### Mall order url

If the mall service is available, you can get the URL of the order of the mall in user's area

**Declaration**

get the URL of the order of the mall in user's area,This method is an asynchronous method.

``` java
requestUserCenterPageUrl(IQueryMallPageUrlCallback callback)
```
**Parameter**

| Parameter                 | Description                     |
| -------------------- | ------------------------ |
| IQueryMallPageUrlCallback | mall order url callbacks |

**Example**
``` java
ITuyaMallSdk iTuyaMallSdk = TuyaOptimusSdk.getManager(ITuyaMallSdk.class);
iTuyaMallSdk.requestUserCenterPageUrl(new IQueryMallPageUrlCallback() {
    @Override
    public void onSuccess(String domain) {
        L.d("requestUserCenterPageUrl", domain);
    }
    @Override
    public void onError(String code, String error) {
        L.e("requestUserCenterPageUrl", error);
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
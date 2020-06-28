# 涂鸦智能 Android 商城业务包 接入指南

## 功能概述

H5 商城业务包提供了承载 「App 商城」的 Android 容器，让您的 App 具备强大的商城能力，让移动端流量通过商城变现。

> 「App 商城」为涂鸦平台提供的一项增值服务，详情可以在涂鸦智能平台 [增值服务](https://www.tuya.com/vas/) 中搜索 「App 商场」了解

## 集成 设备控制业务包
### 创建工程

   在 Android Studio 中建立你的工程,接入公版 SDK 并配置完成

### 根目录的 build.gradle 配置

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

### module 的 build.gradle 配置

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

### 资源配置

  ``` xml
  <style name="AppTheme" parent="Theme.AppCompat.Light.NoActionBar">
      <item name="app_bg_color">#FFFFFF</item>
  </style>
  ```

### 混淆配置

  ``` 
  # 应配置 build.gradle 里所有三方依赖混淆

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

### Application 中初始化涂鸦智能商城业务包

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

## 功能调用

### 商城可用性

当前用户所在区的商城业务是否可用

**接口说明**

当前用户所在区商城是否可用，此接口为异步方法

``` java
requestSupportMall(ResultListener<Boolean> listener)
```
**参数说明**

| 参数         | 说明                                                         |
| ------------ | ------------------------------------------------------------ |
| listener     | 商城可用请求异步回调 |

**示例代码**
``` java
ITuyaMallSdk iTuyaMallSdk = TuyaOptimusSdk.getManager(ITuyaMallSdk.class);
iTuyaMallSdk.requestSupportMall(new Business.ResultListener<Boolean>() {
    @Override
    public void onFailure(BusinessResponse businessResponse, Boolean aBoolean, String s) {
            L.e("requestSupportMall", s);
    }
    @Override
    public void onSuccess(BusinessResponse businessResponse, Boolean aBoolean, String s) {
            //aBoolean = true 表示商城可用
            L.d("requestSupportMall", String.valueOf(aBoolean));
    }
});
```

### 商城首页链接

若商城可用时，可获取用户所在区商城首页 URL

**接口说明**

 获取用户所在区商城首页 URL，此接口为异步接口

``` java
requestHomePageUrl(IQueryMallPageUrlCallback callback)
    ```
**参数说明**

| 参数                          | 说明                            |
| ----------------------------- | ------------------------------- |
| IQueryMallPageUrlCallback | 商城首页请求异步回调 |

**示例代码**
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

### 商城订单链接

若商城可用时，可获取用户所在区商城订单 URL

**接口说明**

 获取用户所在区商城订单 URL，此接口为异步接口

``` java
requestUserCenterPageUrl(IQueryMallPageUrlCallback callback)
```
**参数说明**

| 参数                 | 说明                     |
| -------------------- | ------------------------ |
| IQueryMallPageUrlCallback | 商城订单请求异步回调 |

**示例代码**
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

### 打开商城页面

商城展示页面支持 Activity 和 Fragment

**示例代码**

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
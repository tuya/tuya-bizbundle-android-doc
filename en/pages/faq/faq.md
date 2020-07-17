# FAQ BizBundle

## Features Overview

The FAQ BizBundle provides an Android container that hosts "questions and feedback", and provides a troubleshooting and feedback channel for your app.

## Biz Bundle integration

### Create project

Create your project in Android Studio, connect to the public SDK and configure it

### The build.gradle configuration of the root directory

```java
allprojects {
      repositories {
        maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
        maven { url 'https://jitpack.io' }
      }
  }
```

### module's build.gradle configuration

```java
android {
    defaultConfig {
        multiDexEnabled true
    }
  
    packagingOptions {
        pickFirst 'lib/*/libc++_shared.so'
    }
}
dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:28.0.0'
    implementation "com.android.support:support-fragment:28.0.0"
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    implementation "com.android.support:recyclerview-v7:28.0.0"
    implementation 'io.reactivex.rxjava2:rxandroid:2.1.1'
    implementation 'io.reactivex.rxjava2:rxjava:2.2.9'

    implementation "com.tuya.smart:tuyasmart-demo-login:3.17.6r141"
    implementation 'com.tuya.smart:tuyasmart:3.17.6-beta2'
    implementation "com.tuya.smart:tuyasmart-framework:3.17.0.3r139-external"
    implementation 'com.tuya.smart:tuyasmart-stencilwrapper:3.17.0.2r139'
    implementation "com.tuya.smart:tuyasmart-base:3.17.0r139-rc.3"
    implementation 'com.tuya.smart:tuyasmart-uispecs:0.0.9'

    implementation 'com.facebook.fresco:fresco:1.13.0'
    implementation 'com.alibaba:fastjson:1.1.67.android'
    implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'

    implementation 'com.tuya.smart:tuyasmart-stencilmodel:3.17.0r139-rc.2'
    implementation "com.tuya.android.module:tymodule-annotation:0.0.8"

    implementation 'com.tuya.smart:tuyasmart-webcontainer:3.17.6r141-open-rc.2'
    implementation 'com.tuya.smart:tuyasmart-appshell:3.10.0'
    implementation 'com.tuya.smart:tuyasmart-xplatformmanager:1.0.0'
    implementation 'com.tuya.smart:tuyasmart-picture:3.12.0r123'


    implementation 'com.tuya.smart:tuyasmart-feedback-api:3.17.6r141-rc.1'
    implementation 'com.tuya.smart:tuyasmart-feedback:3.17.6r141-rc.1'
    implementation 'com.tuya.smart:tuyasmart-rpc:3.12.0r123'

    implementation "com.tuya.android:net-diagnosis:2.6.0-rc.1"
    implementation 'com.tuya.smart:tuyasmart-netdiagnosis:3.11.7r121-rc.1'
    implementation 'com.tuya.smart:tuyasmart-netdiagnosisApi:3.11.7r121-rc.1'

    implementation 'org.jetbrains.kotlin:kotlin-stdlib:1.3.72'
}
```

### Configure Theme

```java
    <style name="Default_Public_Theme" parent="AppTheme">
        <item name="app_bg_color">@color/app_bg_color</item>
        <item name="list_primary_color">@color/list_primary_color</item>
        <item name="list_sub_color">@color/list_sub_color</item>
    </style>
```

### Configure resources

```java
    <color name="app_bg_color">#F2F2F2</color>
    <color name="list_primary_color">#303030</color>
    <color name="list_sub_color">#626262</color>
    <color name="color_CC4600">#cc4600</color>
    <color name="color_bdbdbd">#bdbdbd</color>
```

### Init FAQ Biz Bundle in the Application

```java
public class TuyaApplication extends Application {
    @Override
    public void onCreate() {
        super.onCreate();
        TuyaSdk.init(this);
        TuyaWrapper.init(this, new RouteEventListener() {
            @Override
            public void onFaild(int errorCode, UrlBuilder urlBuilder) {

            }
        });
    }
}
```

## Operating Guide

### Attention

1.Make sure that the user is logged in before using any interface

2.When the login user changes, be sure to re-judge the FAQ availability status and re-acquire the FAQ page



## Go To FAQ Page

* Service scheme

```java
FeedbackService feedbackService = MicroContext.findServiceByInterface(FeedbackService.class.getName());
if (feedbackService != null) {
  feedbackService.jumpToWebHelpPage(MainActivity.this);
}
```

* Routing scheme

```java
UrlRouter.execute(UrlRouter.makeBuilder(MainActivity.this, "helpCenter"));
```


# Message Center BizBundle

### Introduction

The message center business bundle provides the business logic of the message center of Tuya APP. The business functions mainly cover the push historical records of various types of messages, mainly including three categories of alarms, homes, and notifications. The alarms include device alarms, scene automation and other execution records.
The setting page of the message center can enable or disable various types of message push. For alarm messages, it can support adding no-disturb periods to the device.

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
    implementation "com.android.support:recyclerview-v7:28.0.0"
    implementation "com.android.support:support-fragment:28.0.0"
    implementation "com.android.support:viewpager:28.0.0"
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'
    implementation "android.arch.lifecycle:extensions:1.1.1"

    implementation "com.tuya.smart:tuyasmart-base:3.17.0r139-rc.3"
    implementation 'com.tuya.smart:tuyasmart:3.17.6-beta2'
    implementation "com.tuya.smart:tuyasmart-framework:3.17.0.3r139-external"
    implementation 'com.tuya.android:dimencompat:1.0.1'
    implementation 'com.tuya.smart:tuyasmart-uispecs:0.0.9'
    implementation 'com.tuya.smart:tuyasmart-uiadapter:3.13.3r129-rc.4'
    implementation 'com.tuya.smart:tuyasmart-stencilwrapper:3.17.0.2r139'

    implementation "jp.wasabeef:recyclerview-animators:2.2.4"
    implementation 'org.jetbrains.kotlin:kotlin-stdlib:1.3.72'
    implementation 'com.facebook.fresco:fresco:1.13.0'
    implementation 'com.alibaba:fastjson:1.1.67.android'
    implementation 'com.squareup.okhttp3:okhttp-urlconnection:3.12.3'
    implementation 'io.reactivex.rxjava2:rxandroid:2.1.1'

    implementation 'com.tuya.smart:tuyasmart-video:3.12.6r125'
    implementation 'com.tuya.smart:tuyasmart-imagepipeline-okhttp3:0.0.1'
    implementation 'com.tuya.smart:tuya-commonbiz-api:1.0.0-SNAPSHOT'
    implementation 'com.tuya.android.module:tymodule-annotation:0.0.8'
    implementation 'com.tuya.smart:tuyasmart-stencilmodel:3.17.0r139-rc.2'
      
    implementation 'com.tuya.smart:tuyasmart-panel:3.17.6r141-open'
    implementation 'com.facebook.react:react-native:0.51.1.11'
    
    implementation 'com.tuya.smart:tuyasmart-message-api:3.17.6r141-rc.1'
    implementation 'com.tuya.smart:tuyasmart-message:3.17.6r141-rc.3'
    implementation 'com.tuya.smart:tuyasmart-panel-reactnative:3.17.6r141.6-open'
}
```

### Configure Theme

```java
    <style name="Default_Public_Theme" parent="AppTheme">
        <item name="app_bg_color">@color/app_bg_color</item>
        <item name="list_primary_color">@color/list_primary_color</item>
        <item name="list_sub_color">@color/list_sub_color</item>
        <item name="list_line_color">@color/list_line_color</item>
    </style>
```

### Configure resources

```java
    <attr name="layout_flexBasisPercent" format="fraction"/>
      
    <color name="app_bg_color">#F2F2F2</color>
    <color name="list_primary_color">#303030</color>
    <color name="list_sub_color">#626262</color>
    <color name="color_CC4600">#cc4600</color>
    <color name="color_bdbdbd">#bdbdbd</color>
    <color name="list_line_color">#DBDBDB</color>
```

### Init Message Center Biz Bundle in the Application

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
        FrescoManager.initFresco(this);
    }
}
```

## Operating Guide

### Attention

1.Make sure that the user is logged in before using any interface

2.When the login user changes, be sure to re-judge the Message Center availability status and re-acquire the Message Center page

3.To display the encrypted picture, FrescoManager.initFresco(this) must be called, and the initialization cannot be repeated, otherwise the picture cannot be decrypted

## Go To Message Center Page

* StartActivity scheme

```java
ActivityUtils.gotoActivity(MainActivity.this,
                        MessageContainerActivity.class,
                        ActivityUtils.ANIMATE_SLIDE_TOP_FROM_BOTTOM,
                        false);
```

* Routing scheme

```java
UrlRouter.execute(new UrlBuilder(MainActivity.this, "messageCenter"));
```
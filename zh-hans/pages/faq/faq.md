# 常见问题与反馈业务包

## 功能介绍

常见问题与反馈业务包提供了承载 「问题与反馈」的 Android 容器，为您的App提供了问题排查与反馈渠道。

## 业务包集成

### 创建工程

在 Android Studio 中建立你的工程,接入公版 SDK 并配置完成

### 根目录的 build.gradle 配置

```java
allprojects {
      repositories {
        maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
        maven { url 'https://jitpack.io' }
      }
  }
```

### module 的 build.gradle 配置

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

### Theme配置

```java
    <style name="Default_Public_Theme" parent="AppTheme">
        <!-- 菜单标题文字颜色 -->
        <item name="app_bg_color">@color/app_bg_color</item>
        <item name="list_primary_color">@color/list_primary_color</item>
        <item name="list_sub_color">@color/list_sub_color</item>
    </style>
```

### 资源配置

```java
    <color name="app_bg_color">#F2F2F2</color>
    <color name="list_primary_color">#303030</color>
    <color name="list_sub_color">#626262</color>
    <color name="color_CC4600">#cc4600</color>
    <color name="color_bdbdbd">#bdbdbd</color>
```

### Application中初始化常见问题与反馈业务包

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

## 使用指南

### 注意事项

1.在调用任何接口之前，务必确认用户已登录；

2.登录用户发生状态变化时，务必重新判断常见问题与反馈业务包可用状态并重新获取常见问题与反馈页面。



## 进入常见问题与反馈页面

* 服务化方案

```java
FeedbackService feedbackService = MicroContext.findServiceByInterface(FeedbackService.class.getName());
if (feedbackService != null) {
  feedbackService.jumpToWebHelpPage(MainActivity.this);
}
```

* 路由方案

```java
UrlRouter.execute(UrlRouter.makeBuilder(MainActivity.this, "helpCenter"));
```


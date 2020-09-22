# 消息中心业务包

## 功能介绍

消息中心业务包提供涂鸦APP消息中心业务逻辑。业务功能主要涵盖各类消息的推送历史记录，主要包括告警、家庭、通知三个消息大类。其中告警包含设备告警，场景自动化等执行记录。
消息中心设置页可以启用或关闭各类消息推送，对于告警类消息可以支持对设备添加免打扰时段。

## 业务包集成

### 创建工程

在 Android Studio 中建立你的工程,接入公版 SDK 并配置完成


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
    implementation 'com.tuya.smart:tuya-commonbiz-api:1.0.0'
    implementation 'com.tuya.android.module:tymodule-annotation:0.0.8'
    implementation 'com.tuya.smart:tuyasmart-stencilmodel:3.17.0r139-rc.2'
    
    implementation 'com.tuya.smart:tuyasmart-panel:3.17.6r141-open'
    implementation 'com.tuya.smart:react-native:0.51.1.11'
      
    implementation 'com.tuya.smart:tuyasmart-message:3.17.6r141-rc.2'
    implementation 'com.tuya.smart:tuyasmart-message-api:3.17.6r141-rc.1'
}
```

### Theme配置

```java
    <style name="Default_Public_Theme" parent="AppTheme">
        <!-- 菜单标题文字颜色 -->
        <item name="app_bg_color">@color/app_bg_color</item>
        <item name="list_primary_color">@color/list_primary_color</item>
        <item name="list_sub_color">@color/list_sub_color</item>
        <item name="list_line_color">@color/list_line_color</item>
    </style>
```

### 资源配置

```java
    <attr name="layout_flexBasisPercent" format="fraction"/>
      
    <color name="app_bg_color">#F2F2F2</color>
    <color name="list_primary_color">#303030</color>
    <color name="list_sub_color">#626262</color>
    <color name="color_CC4600">#cc4600</color>
    <color name="color_bdbdbd">#bdbdbd</color>
    <color name="list_line_color">#DBDBDB</color>
```

### Application中初始化消息中心业务包

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

## 使用指南

### 注意事项

1.在调用任何接口之前，务必确认用户已登录；

2.登录用户发生状态变化时，务必重新判断消息中心可用状态并重新获取消息中心页面；

3、显示加密图片，需调用FrescoManager.initFresco(this) ,不能重复初始化，否则无法进行图片解密。

## 进入消息中心

* StartActivity方法

```java
ActivityUtils.gotoActivity(MainActivity.this,
                        MessageContainerActivity.class,
                        ActivityUtils.ANIMATE_SLIDE_TOP_FROM_BOTTOM,
                        false);
```

* 路由方法

```java
UrlRouter.execute(new UrlBuilder(MainActivity.this, "messageCenter"));
```
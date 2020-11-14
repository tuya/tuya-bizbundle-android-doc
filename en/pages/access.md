# Integration

UI BizBundle General Profiles

## Summary

Bizbundle support minSdkVersion of 19,targetSdkVersion of 29,only Anroidx builds are supported.

## Bizbundle Profiles

### assets profile

[module_app.json](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/tree/main/assets/) as a serviced profiles,Copy module_app.json to the assets folder under the app directory.[required]

[x_platform_config.json](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/tree/main/assets/) for Mall and Device Control Bizbundle serviced profiles，Copy x_platform_config.json to the assets folder under the app directory。[optional]

### res profile

[res/values/ty_config.xml](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/blob/main/res/values/)bizbundle profiles and default colors[required]

[res/values](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/blob/main/res/values/) multilingual resource files [optional]

### style profile

[res/values/styles.xml](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/blob/main/res/values/)bizbundle theme [required]

``` xml
    <!-- Base application theme. -->
    <style name="Base_BizBundle_Theme" parent="AppTheme">
        <item name="status_font_color">@color/status_font_color</item>
        <item name="status_bg_color">@color/status_bg_color</item>
        <item name="navbar_font_color">@color/navbar_font_color</item>
        <item name="navbar_bg_color">@color/navbar_bg_color</item>
        <item name="app_bg_color">@color/app_bg_color</item>
        <item name="fragment_bg_color">@color/app_bg_color</item>
        <item name="list_primary_color">@color/list_primary_color</item>
        <item name="list_sub_color">@color/list_sub_color</item>
        <item name="list_secondary_color">@color/list_secondary_color</item>
        <item name="list_line_color">@color/list_line_color</item>
        <item name="list_bg_color">@color/list_bg_color</item>
        <item name="primary_button_font_color">@color/primary_button_font_color</item>
        <item name="primary_button_bg_color">@color/primary_button_bg_color</item>
        <item name="secondary_button_font_color">@color/secondary_button_font_color</item>
        <item name="secondary_button_bg_color">@color/secondary_button_bg_color</item>
        <item name="notice_font_color">@color/notice_font_color</item>
        <item name="notice_bg_color">@color/notice_bg_color</item>
        <item name="bg_normal_text_bt">@drawable/bg_text_bts</item>
        <item name="app_name">@string/app_name</item>
        <item name="is_splash_used">false</item>
        <item name="ap_default_ssid">@string/ap_mode_ssid</item>
        <item name="ap_connect_description">@string/ty_ap_connect_description</item>
        <item name="is_scan_support">@bool/is_scan_support</item>
        <item name="is_need_blemesh_support">@bool/is_need_blemesh_support</item>
        <item name="status_bg_color_75">@color/status_bg_color_75</item>
        <item name="status_bg_color_90">@color/status_bg_color_90</item>
    </style>
```


### AndroidManifest.xml profile
``` xml
    <!-- required -->
    <application
        android:supportsRtl="false"
        android:theme="@style/Base_BizBundle_Theme"
        tools:replace="android:theme"/>
```

### dependent source configuration
root build.gradle 
``` java
allprojects {
    repositories {
        jcenter()
        maven { url 'https://jitpack.io' }
    }
}
```
app build.gradle
``` java
android {
    packagingOptions {
        pickFirst 'lib/*/libc++_shared.so'
        pickFirst 'lib/*/libgnustl_shared.so'
    }
  
    lintOptions {
        abortOnError false
    }

    defaultConfig {
        ndk { abiFilters "armeabi-v7a", "arm64-v8a" }
    }

    compileOptions {
        sourceCompatibility 1.8
        targetCompatibility 1.8
    }
}
```

### Application init

Bizbundle initialization generic part

``` java
    // Please do not change the initialization order.
    Fresco.initialize(this);
    // SDK init
    TuyaHomeSdk.init(this);

    // bizbundle init
    TuyaWrapper.init(this, new RouteEventListener() {
        @Override
        public void onFaild(int errorCode, UrlBuilder urlBuilder) {
                Log.e("router not implement", urlBuilder.target);
        }
    }, new ServiceEventListener() {
        @Override
        public void onFaild(String serviceName) {
                Log.e("service not implement", serviceName);
        }
    });
    TuyaOptimusSdk.init(this);

    // register family service
    TuyaWrapper.registerService(AbsBizBundleFamilyService.class, new BizBundleFamilyServiceImpl());
```

### login and logout
After the bizbundle is accessed, the following methods should be invoked at account login and logout, respectively
``` java
    //login
    TuyaWrapper.onLogin();
    //logout
    TuyaWrapper.onLogout(Context context);
```

### implement family service

Set the current family homeId by inheriting the AbsBizBundleFamilyService abstract class.

**sample code**
``` java
public class BizBundleFamilyServiceImpl extends AbsBizBundleFamilyService {

    private long mHomeId;

    @Override
    public long getCurrentHomeId() {
        return mHomeId;
    }

    @Override
    public void setCurrentHomeId(long homeId) {
        mHomeId = homeId;
    }
}
```

### set family HomeId

After fetching the family list, set the family homeId with a service call

**sample code**
``` java
    AbsBizBundleFamilyService service = MicroServiceManager.getInstance().findServiceByInterface(AbsBizBundleFamilyService.class.getName());
    //Set the homeId to the current home
    service.setCurrentHomeId(homeBean.getHomeId());
```
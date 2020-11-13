# 框架接入

业务包通用配置说明

## 版本说明

业务包支持 minSdkVersion 为 19，targetSdkVersion 为 29，仅支持 Anroidx 构建。

## 业务包配置文件

### assets 配置

[module_app.json](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/tree/main/assets/) 和 [x_platform_config.json](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/tree/main/assets/) 为服务化配置文件，请拷贝至 app 目录下的 assets 文件夹下即可生效。[必选]

### res 配置

[res/values/ty_config.xml](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/blob/main/res/values/)业务包配置文件及默认颜色[必选]

[res/values](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/blob/main/res/values/)多语言资源文件，可根据情况选择配置 [可选]

### style 配置

[res/values/styles.xml](https://github.com/tuya/tuya-ui-bizbundle-android-config-values/blob/main/res/values/)业务包主题文件[必选]

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

### AndroidManifest.xml 配置
``` xml
    <!-- 必选配置 -->
    <application
        android:supportsRtl="false"
        android:theme="@style/Base_BizBundle_Theme"
        tools:replace="android:theme"/>
```

### 依赖源配置
根目录的 build.gradle 文件
``` java
allprojects {
    repositories {
        jcenter()
        maven { url 'https://jitpack.io' }
    }
}
```
app 的 build.gradle 文件
只支持 arm 架构的 cpu 版本
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
### Application 初始化
业务包初始化通用部分
``` java
    // 请不要修改初始化顺序
    Fresco.initialize(this);
    // SDK 初始化
    TuyaHomeSdk.init(this);

    // 业务包初始化
    TuyaWrapper.init(this, new RouteEventListener() {
        @Override
        public void onFaild(int errorCode, UrlBuilder urlBuilder) {
                // 路由未实现回调
                Log.e("router not implement", urlBuilder.target);
        }
    }, new ServiceEventListener() {
        @Override
        public void onFaild(String serviceName) {
                // 服务未实现回调
                Log.e("service not implement", serviceName);
        }
    });
    TuyaOptimusSdk.init(this);

    // 注册家庭服务，商城业务包可以不注册此服务
    TuyaWrapper.registerService(AbsBizBundleFamilyService.class, new BizBundleFamilyServiceImpl());
```
### 登录和退出
业务包接入后，应在账号登录和退出时分别调用如下方法
``` java
    //登录
    TuyaWrapper.onLogin();
    //退出
    TuyaWrapper.onLogout(Context context);
```
### 实现家庭服务

通过继承 AbsBizBundleFamilyService 抽象类，实现设置当前家庭 homeId

**示例代码**
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

### 设置家庭 HomeId

当获取家庭列表后，通过服务化调用设置家庭 homeId

**示例代码**
``` java
    AbsBizBundleFamilyService service = MicroServiceManager.getInstance().findServiceByInterface(AbsBizBundleFamilyService.class.getName());
    //设置为当前家庭的homeId
    service.setCurrentHomeId(homeBean.getHomeId());
```
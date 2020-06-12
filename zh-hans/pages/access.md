# 框架接入

## 业务包配置文件

### assets 配置

**module_app.json** 为服务化配置文件，拷贝 module_app.json 至 app 目录下的 assets 文件夹下即可生效。[必选]

**x_platform_config.json** 为商场和设备控制业务包服务化配置文件，在接入商场和设备控制业务包时拷贝 x_platform_config.json 至 app 目录下的 assets 文件夹下即可生效。[可选]

### res 配置

**res/vaules/colors.xml** 为业务包所需颜色资源文件 [必选]

**res/values/string.xml** 为业务包自定义配置和默认语言资源文件 [必选]

**res/values**目录下其他文件夹为多语言资源文件，可根据情况选择配置 [可选]

### 依赖源配置
根目录的 build.gradle 文件
``` java
allprojects {
    repositories {
        maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
    }
}
```
app 的 build.gradle 文件
``` java
android {
    defaultConfig {
        ndk {
            abiFilters "armeabi-v7a", "arm64-v8a"
    }
}
```
Application 初始化
``` java 
@Override
public void onCreate() {
    super.onCreate();
    TuyaWrapper.init(this);
    TuyaHomeSdk.init(this);
    TuyaOptimusSdk.init(this);
}
```



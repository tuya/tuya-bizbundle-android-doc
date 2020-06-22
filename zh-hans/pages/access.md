# 框架接入

## 业务包配置文件

### assets 配置

[module_app.json](https://github.com/TuyaInc/tuya_smart_bizbundle_language/blob/master/assets/) 为服务化配置文件，拷贝 module_app.json 至 app 目录下的 assets 文件夹下即可生效。[必选]

[x_platform_config.json](https://github.com/TuyaInc/tuya_smart_bizbundle_language/blob/master/assets/) 为商场和设备控制业务包服务化配置文件，在接入商场和设备控制业务包时拷贝 x_platform_config.json 至 app 目录下的 assets 文件夹下即可生效。[可选]

### res 配置

[res/values](https://github.com/TuyaInc/tuya_smart_bizbundle_language/tree/master/res)多语言资源文件，可根据情况选择配置 [可选]

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


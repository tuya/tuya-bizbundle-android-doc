# Integration

**[NOTE]**

>  We have currently deprecated maven { url 'https://maven-other.tuya.com/repository/maven-public/' } using jcenter()

>  Please remove the SNAPSHOT suffix for components using the release version.

## BizBundle Profiles

### assets profile

[module_app.json](https://github.com/TuyaInc/tuya_smart_bizbundle_language/blob/master/assets/) as a serviced profiles,Copy module_app.json to the assets folder under the app directory.[required]

[x_platform_config.json](https://github.com/TuyaInc/tuya_smart_bizbundle_language/blob/master/assets/) for Mall and Device Control Bizbundle serviced profiles，Copy x_platform_config.json to the assets folder under the app directory。[optional]

### res profile

[res/values](https://github.com/TuyaInc/tuya_smart_bizbundle_language/tree/master/res) multilingual resource files [optional]

### dependent source configuration
root build.gradle 
``` java
allprojects {
    repositories {
            jcenter()
    }
}
```
app build.gradle
``` java
android {
    defaultConfig {
        ndk {
            abiFilters "armeabi-v7a", "arm64-v8a"
    }
}
```


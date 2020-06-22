# Integration

## BizBundle Profiles

### assets profile

**module_app.json** as a serviced profiles,Copy module_app.json to the assets folder under the app directory.[required]

**x_platform_config.json** for Mall and Device Control Bizbundle serviced profiles，Copy x_platform_config.json to the assets folder under the app directory。[optional]

### res profile

**res/values** multilingual resource files [optional]

### dependent source configuration
root build.gradle 
``` java
allprojects {
    repositories {
        maven { url 'https://maven-other.tuya.com/repository/maven-public/' }
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


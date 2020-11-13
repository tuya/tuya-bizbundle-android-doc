#  H5 商城业务包

## 功能概述

H5 商城业务包提供了承载 「App 商城」的 Android 容器，让您的 App 具备强大的商城能力，让移动端流量通过商城变现。

> 「App 商城」为涂鸦平台提供的一项增值服务，详情可以在涂鸦智能平台 [增值服务](https://www.tuya.com/vas/) 中搜索 「App 商场」了解

## 集成 商城业务包

### 创建工程

   在 Android Studio 中建立你的工程,接入公版 SDK，并完成业务包[框架接入](./access.md)

### module 的 build.gradle 配置

  ``` groovy
	dependencies {
        api 'com.tuya.smart:tuyasmart-bizbundle-mall:3.20.0-5'
  	}
  ```

### 混淆配置

  ``` 
  # 应配置 build.gradle 里所有三方依赖混淆

  #fastJson
  -keep class com.alibaba.fastjson.**{*;}
  -dontwarn com.alibaba.fastjson.**    

  -keep class com.squareup.okhttp.** { *; }
  -keep interface com.squareup.okhttp.** { *; }
  -dontwarn com.squareup.okhttp.**    

  -keep class okio.** { *; }
  -dontwarn okio.**    
  
  -keep class com.tuya.**{*;}
  -dontwarn com.tuya.**
  ```

## 功能调用

### 商城可用性

当前用户所在区的商城业务是否可用

**接口说明**

当前用户所在区商城是否可用

``` java
isSupportMall()
```
**示例代码**
``` java
    TuyaMallService service = MicroContext.getServiceManager().findServiceByInterface(TuyaMallService.class.getName());
    boolean mallEnable = service.isSupportMall()
```

### 商城首页链接

若商城可用时，可获取用户所在区商城首页 URL

**接口说明**

 获取用户所在区商城首页 URL，此接口为异步接口

``` java
requestMallHome(IGetMallUrlCallback callback)
```
**参数说明**

| 参数                          | 说明                            |
| ----------------------------- | ------------------------------- |
| IGetMallUrlCallback | 商城首页请求异步回调 |

**示例代码**
``` java
    TuyaMallService service = MicroContext.getServiceManager().findServiceByInterface(TuyaMallService.class.getName());
    service.requestMallHome(new IGetMallUrlCallback() {
        @Override
        public void onSuccess(String url) {
            Log.i("mall url = ",url);
        }
        @Override
        public void onError(String code, String error) {
        }
    });
```

### 商城订单链接

若商城可用时，可获取用户所在区商城订单 URL

**接口说明**

 获取用户所在区商城订单 URL，此接口为异步接口

``` java
requestMallUserCenter(IGetMallUrlCallback callback)
```
**参数说明**

| 参数                 | 说明                     |
| -------------------- | ------------------------ |
| IGetMallUrlCallback | 商城订单请求异步回调 |

**示例代码**
``` java
    TuyaMallService service = MicroContext.getServiceManager().findServiceByInterface(TuyaMallService.class.getName());
    service.requestMallUserCenter(new IGetMallUrlCallback() {
        @Override
        public void onSuccess(String url) {
            Log.i("mall user center url = ", url);
        }
        @Override
        public void onError(String code, String error) {
        }
    });
```

### 打开商城页面

商城展示页面支持 Activity 和 Fragment

**示例代码**

Activity
``` java
Intent intent = new Intent(context, WebViewActivity.class);
intent.putExtra("Uri", url);
context.startActivity(intent);
```

Fragment
``` java
WebViewFragment fragment = new WebViewFragment();
Bundle args = new Bundle();
args.putString("Uri", url);
args.putBoolean("enableLeftArea", true);
fragment.setArguments(args);
getSupportFragmentManager().beginTransaction()
        .add(R.id.web_content, fragment, WebViewFragment.class.getSimpleName())
        .commit();
```
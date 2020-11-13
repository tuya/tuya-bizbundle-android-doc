# 常见问题与反馈业务包

## 功能介绍

常见问题与反馈业务包提供了承载 「问题与反馈」的 Android 容器，为您的App提供了问题排查与反馈渠道。

## 业务包集成

### 创建工程

在 Android Studio 中建立你的工程,接入公版 SDK 并完成业务包[框架接入](./access.md)


### module 的 build.gradle 配置

```java

dependencies {
    api 'com.tuya.smart:tuyasmart-bizbundle-feedback:3.20.0-5'
}
```


### 跳转未实现路由地址

[通过路由回调跳转到实现页面](./access.md#application-初始化)

## 进入常见问题与反馈页面

路由方案

```java
UrlRouter.execute(UrlRouter.makeBuilder(MainActivity.this, "helpCenter"));
```

### 注意事项

1.在调用任何接口之前，务必确认用户已登录；

2.登录用户发生状态变化时，务必重新判断常见问题与反馈业务包可用状态并重新获取常见问题与反馈页面。

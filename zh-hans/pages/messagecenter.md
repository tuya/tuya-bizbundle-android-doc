# 消息中心业务包

## 功能介绍

消息中心业务包提供涂鸦APP消息中心业务逻辑。业务功能主要涵盖各类消息的推送历史记录，主要包括告警、家庭、通知三个消息大类。其中告警包含设备告警，场景自动化等执行记录。
消息中心设置页可以启用或关闭各类消息推送，对于告警类消息可以支持对设备添加免打扰时段。

## 业务包集成

### 创建工程

在 Android Studio 中建立你的工程,接入公版 SDK 并完成业务包[框架接入](./access.md)

### module 的 build.gradle 配置

```java

dependencies {
    api 'com.tuya.smart:tuyasmart-bizbundle-message:3.20.0-5'
}
```
### 跳转未实现路由地址
[通过路由回调跳转到实现页面](./access.md#application-初始化)


## 进入消息中心

```java
UrlRouter.execute(new UrlBuilder(MainActivity.this, "messageCenter"));
```
### 注意事项

1.在调用任何接口之前，务必确认用户已登录；

2.登录用户发生状态变化时，务必重新判断消息中心可用状态并重新获取消息中心页面；
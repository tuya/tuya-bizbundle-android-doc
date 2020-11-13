# 涂鸦 Android 业务包

## 概述

涂鸦 Android 业务包 (Tuya Android BizBundle)  是指包含业务逻辑和 UI 界面的涂鸦垂直业务模块，旨在为基于涂鸦全屋智能 SDK 开发的客户提供快速一站式接入涂鸦业务模块的能力。

目前提供的业务包 包括：

- H5 商城
- 设备配网
- 设备控制
- 摄像机业务包
- 场景业务包
- 常见问题与反馈
- 消息中心

## 架构设计
涂鸦 Android 业务包以服务化的方式开放，一切功能接入皆以 Service 的方式提供。

![图片](../images/tuya_smart_bizbundle_android.png)

### 获取服务

通过 `BizCore` 获取某业务包提供的服务协议 `Service` 实现，之后调用其服务方法，以达到业务目的

```mermaid
classDiagram
    Service <|.. BizBundle : 业务包实现服务协议
    Service .. BizCore
    BizCore <.. YourClass : 获取实现服务协议的实例
    class Service{
        +doSomeThing()
    }
    class BizCore{
        +findServiceByInterface() id~Service~
    }
    class BizBundle{
    }
    class YourClass{
        +id~Service~ serviceImpl
    }
```

### 实现服务

有些业务包依赖的服务协议并没有实现方（例如面板业务包某些情况下会依赖配网服务，在未接入配网业务包的情况下，该服务协议无人实现），这时你可以创建自己的类来实现对应服务协议，并将它注册给 `BizCore` 来完善业务包功能

```mermaid
classDiagram
    Service <|.. YourClass : 实现服务协议
    BizCore <.. YourClass : 注册你的服务实现类或实例
    Service .. BizCore
    BizCore <.. BizBundle
    class BizBundle{
    }
    class Service{
        +doSomeThing()
    }
    class BizCore{
        +registerService(Protocol, Class)
    }
    class YourClass{
        +doSomeThing()
    }
```



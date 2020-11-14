# Tuya Android UI BizBundle

## Overview

Tuya Android UI BizBundle is a vertical business bundle that contains logic modules and UI pages. It's designed to provide customers with the ability to quickly access the Tuya business module based on the Tuya Smart Home SDK.

Tuya Android BizBundle currently offered includes:
- H5 Mall
- Device Configuration
- Deivce Control
- IPC BizBundle
- Scene BizBundle
- FAQ BizBundle
- Message Center BizBundle

## Architecture

Tuya Android UI BizBundle is opened in a service-oriented manner, and all function access is provided in the form of `Service`

![image](../images/tuya_smart_bizbundle_android.png)

### Get Service

Obtain the instance which implement the service protocol provided by the BizBundle through BizCore, then call its service method to achieve the business purpose

```mermaid
classDiagram
    Service <|.. BizBundle : BizBundle implement servcie protocol
    Service .. BizCore
    BizCore <.. YourClass : Get instance of service
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

### Provide Service

Some BizBundle depend on services that are not implemented, At this time, you can provide your own class to implement the corresponding service, and register it to `BizCore` to improve the BizBundle functions

```mermaid
classDiagram
    Service <|.. YourClass : implement servcie protocol
    BizCore <.. YourClass : Register your service implement class or instance
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
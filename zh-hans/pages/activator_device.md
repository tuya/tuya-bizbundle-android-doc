# 配网业务包

## 功能介绍

业务功能涵盖了目前涂鸦智能所有的 Wi-Fi 设备、ZigBee 设备、蓝牙设备以及支持二维码扫码的设备（例如 GPRS & NB-IOT 设备）等不同类型设备配网前置操作引导和具体入网激活实现

### Wi-Fi 设备配网

支持 Wi-Fi 智能设备入网连接云服务，Wi-Fi 设备配网主要有 EZ 模式和 AP 模式两种，其中 IPC 设备支持扫二维码方式配网

| 名词       | 解释                                                     |
| --------   | ------------------------------------------------------- |
| EZ 模式      | 又称快连模式，APP 把配网数据包打包到802.11数据包的指定区域中，<br> 发送到周围环境；智能设备的 WiFi 模块处于混杂模式下，<br> 监听捕获网络中的所有报文，按照约定的协议数据格式解析出 APP 发出配网信息包。 |
| AP 模式      | 又称热点模式，手机作为 STA 连接智能设备的热点，双方建立一个 Socket 连接通过约定端口交互数据。 |
| IPC 设备扫码配网      | 摄像头设备通过扫描 APP上的二维码获取配网数据信息 |

### ZigBee 设备配网

支持 ZigBee 网关和子设备配网。

| 名词      | 解释                                                     |
| -------- | -------------------------------------------------------  |
| ZigBee 网关 | 融合 ZigBee 网络中协调器和 Wi-Fi 功能的设备，负责 ZigBee 网络的组建及数据信息存储。 |
| 子设备 | ZigBee 网络中的路由或者终端设备，负责数据转发或者终端控制响应。|

### 蓝牙设备配网

涂鸦蓝牙有三条技术线路，主要包括 SingleBLE、SigMesh、TuyaMesh 以及双模设备。

| 名词  | 解释              |
| ----------- | -----------------  |
| SingleBLE     | 通过蓝牙与手机一对一相连的蓝牙单点设备 |
| SigMesh     | 采用蓝牙技术联盟发布的蓝牙拓扑通信|
| TuyaMesh    | 采用涂鸦自研的蓝牙拓扑通信|
| 双模设备     | 支持多协议的设备，即同时具备 Wi-Fi 能力和 BLE 能力的设备|

### 扫码配网设备

该类设备上电后即连接了涂鸦云服务，APP 通过扫描设备上的二维码（必须是涂鸦云服务支持的二维码规则，支持固件具体接入方式可咨询涂鸦科技相关商务及项目经理）使能设备去涂鸦云激活绑定

| 名词  | 解释              |
| ----------- | -----------------  |
| GPRS 设备    | 采用 GPRS 通信技术接入网络连接云服务的智能设备 |
| NB-IOT 设备  | 采用窄带物联网 (NarrowBand-InternetofThings) 技术的智能设备 |

### 自动发现配网

融合涂鸦智能通用配网技术实现，为用户提供一套快捷配网的功能。

## 业务包集成

### 业务集成

1. 创建工程

  在 Android Studio 中建立你的工程，接入涂鸦全屋智能 SDK 并完成 [业务包架构接入](../pages/access.md) 。
  
2. 配置业务模块的 build.gradle

  ``` groovy
	dependencies { 
    api 'com.tuya.smart:tuyasmart-bizbundle-device_activator:3.20.0-5'
	}

  ```
  
3. 混淆配置

  ```bash
	#fastJson
	-keep class com.alibaba.fastjson.**{*;}
	-dontwarn com.alibaba.fastjson.**
	
	#rx
	-dontwarn rx.**
	-keep class rx.** {*;}
	-keep class io.reactivex.**{*;}
	-dontwarn io.reactivex.**
	-keep class rx.**{ *; }
	-keep class rx.android.**{*;}
	
	#fresco
	-keep class com.facebook.drawee.backends.pipeline.Fresco
	-keep @com.facebook.common.internal.DoNotStrip class *
    -keepclassmembers class * {
    @com.facebook.common.internal.DoNotStrip *;
    }
	
	#tuya
	-keep class com.tuya.**{*;}
	-dontwarn com.tuya.**
  ```
  
## 功能调用

### 功能配置

| 配置项  | 字段名              |  描述        |
| ----------- | -----------------  | ----------------- |
| AP 热点名称配置  | ```<string name="ap_mode_ssid">SmartLife</string>``` | AP 配网支持的热点前缀列表，默认配置：SmartLife|
| 是否支持 BLE  | ```<bool name="is_need_ble_support">true</bool>``` | 是否支持 BLE 设备配网功能，默认配置：true|
| 是否支持 MESH  | ```<bool name="is_need_blemesh_support">true</bool>``` | 是否支持 MESH 设备配网功能，默认配置：true|
| 主题色配置  | ```<color name="primary_button_bg_color">#FF5A28</color>``` | 按钮背景配色|
| 主题色配置  | ```<color name="primary_button_font_color">#FFFFFF</color>``` | 按钮的文字配色|
| 是否支持扫一扫 | ```<bool name="is_scan_support">true</bool>``` | 是否支持列表页面右上角扫描配网功能，默认配置：true |

### 方法调用

```java
  TuyaDeviceActivatorManager.startDeviceActiveAction(activity, homeId, 
		new ITuyaDeviceActiveListener() {
            @Override
            public void onDevicesAdd(List<String> devIds, boolean updateRoomData) {
                
            }
            
            @Override
            public void onRoomDataUpdate() {
                
            }

            @Override
            public void onOpenDevicePanel(String s) {
                
            }

            @Override
            public void onExitConfigBiz() {
                
            }
    	});
```

**入参说明**

| 参数         | 说明 |
| ------------ | -------------------------- |
| activity          | activity |
| homeId          | 家庭 id，详情参考涂鸦全屋智能 SDK [家庭创建](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/zh-hans/resource/HomeManager.html) |

**出参说明**

| 参数         | 说明 |
| ------------ | -------------------------- |
| devIds          | 配网成功的设备 id 列表 |
| updateRoomData          | 房间设备信息是否有变更 |
| onOpenDevicePanel          | 可以打开摸个面板，根据实际业务需要选择实现 |
| onExitConfigBiz          | 未执行配网，主动退出配网业务，根据实际业务需要选择实现 |

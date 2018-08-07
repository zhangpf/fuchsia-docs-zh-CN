<!-- # Fuchsia Wireless Networking -->
# Fuchsia无线网络
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/b6b0ca648187efab5aed3cdc26d96e6c99f2f9ad/the-book/wireless_networking.md)

---
<!-- ## Introduction -->
## 简介

<!-- Fuchia's wireless networking stack intends to provide a compliant non-AP station
implementation of IEEE Std 802.11. It supports hardware with both "full MAC" and
"soft MAC" firmware, in which the MLME layer of the 802.11 spec is implemented
in the firmware and the host OS, respectively. -->

Fuchia的无线网络软件栈旨在提供IEEE 802.11标准的兼容非AP实现，它同时支持具有"[FullMAC](https://wireless.wiki.kernel.org/en/developers/documentation/glossary#fullmac)"和"[SoftMAC](https://wireless.wiki.kernel.org/en/developers/documentation/glossary#softmac)"固件的硬件，其中802.11规范的MLME层分别在固件和主机OS中得以实现。

<!-- ## High-level architecture -->
## 高层次架构
```
                          +------------------+        +------------------+
 Fuchsia service          | Fuchsia netstack |        | Fuchsia Wireless |
                          |                  |        | Network Service  |
                          +------------------+        +------------------+
                              ^                        ^                ^
                              |                        |                |
 fdio/FIDL              ------|------------------------|----------------|-------------------
                              |                        |                |
                              v                        |                v
                          +------------------+         |               +--------------+
                          | Fuchsia ethernet |<--------|-------------->| Fuchsia WLAN |
                          | driver           |         |               | MLME driver  |
 devmgr                   +------------------+         |               +--------------+
                                         ^             |                    ^
                                         |             |                    |
                                         v             v                    v
                                    +-------------------+              +-------------------+
                                    | Driver            |              | Driver            |
                                    | (Full MAC device) |              | (Soft MAC device) |
                                    +-------------------+              +-------------------+
                                                      ^                    ^
                                                       \                  /
 hardware bus                       --------------------\----------------/------------------
 (USB, PCI, etc)                                         \              /
                                                          v            v
                                                     +---------------------+
                                                     | Wireless networking |
 hardware                                            | hardware            |
                                                     +---------------------+
```


<!-- ## Drivers -->
## 驱动
<!-- 
A Full MAC driver relies on the firmware in the wireless hardware to implement
the majority of the IEEE 802.11 MLME functions. -->

FullMAC驱动依赖于无线硬件中的固件来实现大多数IEEE 802.11中的MLME功能。

<!-- 
A Soft MAC driver implements the basic building blocks of communication with the
wireless hardware in order to allow the Fuchsia MLME driver to execute the IEEE
802.11 MLME functions. -->

SoftMAC的驱动实现了与无线硬件通信的基本构建块，允许Fuchsia中的MLME驱动执行IEEE 802.11的MLME功能。

<!-- The Fuchsia MLME driver is a hardware-independent layer that provides state
machines for synchronization, authentication, association, and other wireless
networking state. It communicates with a Soft MAC driver to manage the hardware. -->

Fuchsia的MLME驱动是独立于硬件的层，为同步、身份验证、关联和其他无线网络状态提供了状态机维护，它与SoftMAC驱动进行通信来管理硬件。

<!-- ## WLAN service -->
## WLAN服务

<!-- The Fuchsia Wireless Network Service implements the IEEE 802.11 SME functions
and holds state about all the wireless networks that are available in the
current environment. It is the interface to the hardware (via the drivers) used
by components like System UI. -->
Fuchsia的无线网络服务实现了IEEE 802.11的SME功能，并保留了当前环境中所有可用无线网络的状态，它是系统UI等组件（通过驱动程序）使用硬件的接口。

<!-- ## Relation to the Ethernet stack -->
## 和以太网软件栈的关系

<!-- Either a Full MAC driver or the Fuchsia WLAN MLME driver will expose an Ethernet
device in devmgr. This device will behave as any other Ethernet device, and will
provide data packets to the rest of the system. TBD: whether to use Ethernet II
frames always, or support 802.2 SNAP frames. -->

FullMAC驱动或Fuchsia WLAN的MLME驱动在devmgr中以以太网设备的形式存在。该设备的行为与其他任何以太网设备并无区别，并向系统的其余部分提供数据包。

TBD：考虑是否始终使用Ethernet II的帧，或支持802.2的SNAP帧。

<!-- ## Interfaces -->
## 接口

<!-- The Fuchsia Wireless Network Service will communicate with each hardware device
using a channel to the driver, obtained via ioctl. (Eventually this will be
replaced by FIDL.) Messages exchanged over this channel will encode the
request/response for each action, generally following the IEEE 802.11 MLME SAP
definitions. -->

Fuchsia的无线网络服务通过ioctl操作获得的驱动程序信道，与每个硬件设备进行通信（最终将由FIDL替换）。通过此信道交换的消息将为每个操作的请求/响应进行编码，编码通常遵循IEEE 802.11的MLME SAP定义。

<!-- For Soft MAC devices, the hardware driver and the generic MLME driver will
communicate in-process using a DDK "protocol" for wlan devices. Primitives
exposed through this interface include send, receive, and setting the radio
channel. -->

对于SoftMAC设备，硬件驱动程序和通用MLME驱动程序使用针对wlan设备的DDK“协议”进行进程内通信。通过此接口公开的原语操作包括发送、接收和设置无线电（radio）信道。
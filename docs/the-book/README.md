# Fuchsia is not Linux
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/a774512b9d926ee438a77ddc6a5f362b71e0cc4b/the-book/README.md)

---
<!---
_A modular, capability-based operating system_
--->
_模块化的capability-based操作系统_

<!---
This document is a collection of articles describing the Fuchsia operating system,
organized around particular subsystems. Sections will be populated over time.
--->
本文档是一系列描述Fuchsia操作系统的文章集合，围绕特定子系统而组织，各个章节将随着时间的推移而被填充。

## 目录
<!---
## Zircon Kernel

Zircon is the microkernel underlying the rest of Fuchsia. Zircon
also provides core drivers and Fuchsia's libc implementation.

 - [Concepts][zircon-concepts]
 - [System Calls][zircon-syscalls] / VDSO (libzircon)
 - Boot Sequence
--->

## Zircon内核

Zircon是位于Fuchsia其余部分底层的微内核，Zircon还提供了核心驱动程序和Fuchsia的libc实现。
 - [概念][zircon-concepts]
 - [系统调用][zircon-syscalls]/VDSO（libzircon）
 - 启动顺序

<!--- 
## Zircon Core

 - Device Manager & Device Hosts
 - [Device Driver Model (DDK)][zircon-ddk]
 - [C Library (libc)](libc.md)
 - [POSIX I/O (libfdio)](life_of_an_open.md)
 - [Process Start / ELF Loading (liblaunchpad)](launchpad.md) 
--->

## Zircon核心

 - 设备管理器 & 设备主机
 - [设备驱动开发（DDK）][zircon-ddk]
 - [C系统库（libc）](libc.md)
 - [POSIX I/O（libfdio）](life_of_an_open.md)
 - [进程启动/ELF加载（liblaunchpad）](launchpad.md) 

<!---
## Framework

 - [Core Libraries](core_libraries.md)
 - Application model
   - [Interface definition language (FIDL)][FIDL]
   - Services
   - Environments
 - [Boot sequence](boot_sequence.md)
 - Device, user, and story runners
 - Components
 - [Namespaces](namespaces.md)
 - [Sandboxing](sandboxing.md)
 - [Story][framework-story]
 - [Module][framework-module]
 - [Agent][framework-agent]
 - Links
--->
## Framework框架

 - [核心库](core_libraries.md)
 - 应用模型
   - [接口描述语言（FIDL）][FIDL]
   - 服务
   - 上下文环境
 - [启动顺序](boot_sequence.md)
 - 设备，用户和story runner
 - 组件
 - [命名空间](namespaces.md)
 - [沙箱](sandboxing.md)
 - [Story][framework-story]
 - [Module][framework-module]
 - [Agent][framework-agent]
 - 链接

<!---
## Stroage

 - [Block devices](block_devices.md)
 - [File systems](filesystems.md)
 - Directory hierarchy
 - [Ledger][ledger]
 - Document store
 - Application cache
--->
## 存储

 - [块设备](block_devices.md)
 - [文件系统](filesystems.md)
 - 目录层次结构
 - [Ledger（英文原文）][ledger]
 - 文档存储
 - 应用cache

<!---
## Networking

 - Ethernet
 - [Wireless](wireless_networking.md)
 - [Bluetooth][bluetooth]
 - Sockets
 - HTTP

--->
## 网络

 - 以太网
 - [无线网（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/wireless_networking.md)
 - [蓝牙（英文原文）][bluetooth]
 - Sockets
 - HTTP

<!---
## Graphics

 - [Magma (vulkan driver)][magma]
 - [Escher (physically-based renderer)][escher]
 - [Scenic (compositor)][scenic]
 - [Input manager][input-manager]
 - [View manager][view-manager]
 - [Flutter (UI toolkit)][flutter]
--->
## 图形化

 - [Magma（vulkan驱动）（英文原文）][magma]
 - [Escher（基于物理的渲染器）（英文原文）][escher]
 - [Scenic（合成器）（英文原文）][scenic]
 - [输入管理器（英文原文）][input-manager]
 - [视图管理器（英文原文）][view-manager]
 - [Flutter（UI工具包）（英文原文）][flutter]

<!-- 
## Media

 - Audio
 - Video
 - DRM 
-->

## 媒体

 - 声音
 - 视频
 - 数字版权管理（DRM)

<!-- ## Intelligence

 - Context
 - Agent Framework
 - Suggestions -->

## 智能
 - 上下文
 - 代理框架
 - 建议

<!---
## User interface

 - Device, user, and story shells
 - Stories and modules
--->

## 用户接口
 - 设备，用户和story shell
 - story和模块

<!---
## Backwards compatibility

 - POSIX lite (what subset of POSIX we support and why)
 - Web runtime
--->

## 向后兼容性
 
 - 轻量级POSIX（包括我们支持哪些POSIX的子集合以及其原因）

<!---
## Update and recovery

 - Verified boot
 - Updater
--->

## 更新和恢复
 
 - 验证启动
 - 更新器

[zircon-concepts]: /zircon/docs/concepts.md
[zircon-syscalls]: /zircon/docs/syscalls.md
[zircon-ddk]: /zircon/docs/ddk/overview.md
[FIDL]: /zircon/docs/fidl/index.md
[framework-story]: /peridot/docs/modular/story.md
[framework-module]: /peridot/docs/modular/module.md
[framework-agent]: /peridot/docs/modular/agent.md
[ledger]: https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/README.md
[bluetooth]: https://github.com/fuchsia-mirror/garnet/blob/master/bin/bluetooth/README.md
[magma]:  https://github.com/fuchsia-mirror/garnet/blob/master/lib/magma/
[escher]:  https://github.com/fuchsia-mirror/garnet/blob/master/public/lib/escher/
[scenic]:  https://github.com/fuchsia-mirror/garnet/blob/master/docs/ui_scenic.md
[input-manager]:  https://github.com/fuchsia-mirror/garnet/blob/master/docs/ui_input.md
[view-manager]:  https://github.com/fuchsia-mirror/garnet/blob/master/bin/ui/view_manager/
[flutter]: https://flutter.io/

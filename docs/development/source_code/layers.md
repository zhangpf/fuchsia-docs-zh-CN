<!---
# The Fuchsia layer cake
--->
# 千层Fuchsia蛋糕
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/d6693145a6db79ef6db798683c743e640f7d4f96/development/source_code/layers.md)

---

<!---
Fuchsia is the name of the open source project and the complete technical
artifact produced by the open source project. The name "Fuchsia" appears in many
places throughout the codebase, and will be baked into API names exposed to
third-party developers. The names of the individual layers below (with the
exception of Zircon) are implementation details of how we develop Fuchsia, and
we should avoid baking those names into public APIs.
--->
Fuchsia是本开源项目的项目名和本项目完整技术产生品的名字，所以“Fuchsia”这个名字出现在代码库中的很多地方，同时也会出现在暴露给第三方开发商的API名中。而下面的单独层的名称（除了Zircon）体现我们开发Fuchsia的实现细节，但我们也应避免将这些名称暴露给公共API中。

<!---
## Zircon

Zircon is the operating system's foundation: it mediates hardware access,
implements essential software abstractions over shared resources, and provides a
platform for low-level software development.
--->
## Zircon

Zircon作为Fushsia系统的基础：它负责管理硬件访问，在共享资源上实现基本的软件抽象，并为底层软件开发提供平台。

<!---
For example, Zircon contains the kernel, device manager, most core and
first-party device drivers, and low-level system libraries, such as libc and
launchpad. Zircon also defines the Fuchsia IDL (FIDL), which is the protocol
spoken between processes in the system, as well as backends for C and C++. The
backends for other languages will be added by other layers.
--->
例如，Zircon包含内核，设备管理器，大多数核心的官方设备驱动程序，以及底层的系统库（如libc和launchpad）。 Zircon还定义了Fuchsia IDL（FIDL），它是系统中的进程间通信协议，也作为C和C++的后端。 其他语言的后端将由其他层添加。

<!---
## Garnet

Garnet provides device-level system services for software installation,
administration, communication with remote systems, and product deployment.
--->
## Garnet

Garnet为软件安装，管理，与远程系统的通信以及产品部署提供设备级系统服务。

<!---
For example, Garnet contains the network, media, and graphics services. Garnet
also contains the package management and update system.
--->
例如，Garnet包含了网络、媒体和图形服务。除此之外，Garnet还包含软件包管理和更新系统。

<!---
## Peridot

Peridot provides the services needed to create a cohesive, customizable,
multi-device user experience assembled from modules, stories, agents, entities,
and other components.
--->
## Peridot

Peridot提供所需的服务，以创建由模块、情节、代理、实体和其他组件组成的汇聚可定制的多设备用户体验。

<!---
For example, Peridot contains the device, user, and story runners. Peridot also
contains the ledger and resolver, as well as the context and suggestion engines.
--->
例如，Peridot包含设备、用户和情节runner，还包含了ledger和resolver，以及上下文和建议引擎。

<!---
## Topaz

Topaz augments system functionality by implementing interfaces defined by
underlying layers. Topaz contains four major categories of software: modules,
agents, shells, and runners.
--->
## Topaz

Topaz通过实现底层定义的接口来增强系统功能。Topaz包含四个主要类别的软件：module，agent，shell和runner。

<!---
For example, modules include the calendar, email, and terminal modules, shells
include the base shell and the user shell, agents include the email and chat
content providers, and runners include the Web, Dart, and Flutter runners.
--->
例如，module包括日历，电子邮件和终端模块，shell包括基本shell和用户shell，代理包括电子邮件和聊天内容提供方，runner包括Web、Dart以及Flutter runner。

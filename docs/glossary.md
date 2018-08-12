<!-- # Glossary -->
# 术语表
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/261c798f08dd29ef3c288b5af9a8179f7ffc64a7/glossary.md)

---
<!-- ## Overview -->
## 概览

<!-- The purpose of this document is to provide short definitions for a collection
of technical terms used in the Fuchsia source tree. -->

本文档的目的是，为Fuchsia源码树中使用的技术术语提供简短的定义。

<!-- #### Adding new definitions -->
#### 增加新的定义

<!-- - A definition should be limited to two or three sentences and deliver a
high-level description of a term.
- When another non-trivial technical term needs to be employed as part of the
description, consider adding a definition for that term and linking to it from
the original definition.
- A definition should be complemented by a list of links to more detailed
documentation and related topics. -->

- 定义应限于两到三个句子，并提供术语的高层次描述。
- 当需要使用另一个非平凡的技术术语作为描述的一部分时，请考虑为该术语添加定义并将原始定义链接到该术语中。
- 定义应由更详细的文档和相关主题的链接列表补充。

<!-- ## Terms -->
## 术语

#### **Agent**
<!-- 
A component whose life cycle is not tied to any story, is a singleton in user
scope, and provides services to other components. An agent can be invoked by
other components or by the system in response to triggers like push
notifications. An agent can provide services to components, send and receive
messages, and make proposals to give suggestions to the user. -->

代理(Agent)是生命周期与任何[Story](#story)无关的组件。它是用户范围内的一个单例，并为其他组件提供服务，其他组件或系统可以调用Agent以响应推送通知等触发器。Agent可以为组件提供服务，发送和接收消息，并向用户提供建议等。

#### **AppMgr**

<!-- The Application Manager (AppMgr) is responsible for launching components and
managing the namespaces in which those components run. It is the first process
started in the `fuchsia` job by the [DevMgr](#DevMgr). -->

Application Manager(AppMgr)负责启动组件并管理运行这些组件的命名空间。这是[DevMgr](#devmgr)在`fuchsia`中运行的第一个进程。

#### **Armadillo**

<!-- An implementation of a user shell. -->
用户shell的一个实现。

<!-- - [Source](https://fuchsia.googlesource.com/topaz/+/master/shell/armadillo_user_shell/) -->

- [源码](https://github.com/fuchsia-mirror/topaz/tree/master/shell/armadillo)

#### **Base shell**

<!-- The platform-guaranteed set of software functionality which provides a basic
user-facing interface for boot, first-use, authentication, escape from and
selection of user shells, and device recovery. -->

Base shell是平台保证的软件功能集，提供基本的面向用户的界面，用于启动、首次使用、身份验证、从用户shell中退出和选择用户shell，以及设备恢复。

#### **Component**

<!-- A component is a unit of execution and accounting. It consists of a manifest
file and associated code, which comes from a Fuchsia package. A component runs
in a sandbox, accesses objects via its [namespace](#Namespace) and publishes
objects via its export directory. [Modules](#Module) and [Agents](#Agent) are
examples of components. Components are most commonly distributed inside [Fuchsia
Packages](#fuchsia-package). -->
组件(Component)是运行和统计的单元，它由来自Fuchsia软件包的清单文件和相关代码组成。组件在沙箱中运行，通过其[命名空间](#namespace)访问对象，并通过其导出目录发布对象。[Module](#module)和[Agent](#agent)是组件的示例，组件最常见的是分布在[Fuchsia的软件包](#fuchsia-package)中。

#### **Channel**

<!-- A Channel is the fundamental IPC primitive provided by Zircon.  It is a
bidirectional, datagram-like transport that can transfer small messages
including
[Handles](#Handle). -->
Channel是Zircon提供的基本IPC原语，它是一种双向的类似数据报(datagram)的传输通道，可以传输包含[Handle](#handle)的小型消息。
<!-- - [Channel Overview](https://fuchsia.googlesource.com/zircon/+/master/docs/objects/channel.md) -->
- [Channel概览（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/objects/channel.md)

#### **DevHost**

<!-- A Device Host (DevHost) is a process containing one or more device drivers.  They are
created by the Device Manager, as needed, to provide isolation between drivers for
stability and security. -->
设备宿主机（Device Host, DevHost）是包含一个或多个设备驱动的进程，它们由设备管理器根据需要而创建，以提供驱动程序之间的隔离，达到稳定性和安全性的目的。

#### **DevMgr**
<!-- 
The Device Manager (DevMgr) is responsible for enumerating, loading, and
managing the life cycle of device drivers, as well as low level system tasks
(providing filesystem servers for the boot filesystem, launching [AppMgr](
#AppMgr), and so on). -->
设备管理器（Device Manager, DevMgr）负责枚举、加载驱动和管理驱动的生命周期，和处理低层次系统任务（为引导文件系统提供文件系统服务和启动[AppMgr](#appmgr)等等）。

#### **DDK**

<!-- The Driver Development Kit is the documentation, APIs, and ABIs necessary to build Zircon
Device Drivers.  Device drivers are implemented as ELF shared libraries loaded by Zircon's
Device Manager. -->
驱动程序开发工具包(DDK)是构建Zircon设备驱动所必需的文档、API和ABI。设备驱动通过Zircon设备管理器加载的ELF共享库的形式实现。

<!-- - [DDK includes](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/ddk/include/ddk/) -->
- [DDK头文件](https://github.com/fuchsia-mirror/zircon/tree/master/system/ulib/ddk/include/ddk)

#### **Environment**

<!-- A container for a set of components, which provides a way to manage their
lifecycle and provision services for them. All components in an environment
receive access to (a subset of) the environment's services. -->
环境(Environment)是一组组件的容器，提供管理其生命周期和为其提供服务的方法。环境中的所有组件都可以访问环境提供的服务（的子集）。

#### **Escher**
<!-- 
Graphics library for compositing user interface content. Its design is inspired
by modern real-time and physically based rendering techniques though we
anticipate most of the content it renders to have non-realistic or stylized
qualities suitable for user interfaces. -->
Escher是用于合成用户界面内容的图形库，它的设计灵感来自于现代实时的和基于物理的渲染技术，尽管我们预计它呈现的大部分内容都具有适合用户界面的非现实或风格化的质量。

#### **FAR**

<!-- The Fuchsia Archive Format is a container for files to be used by Zircon and Fuchsia. -->
Fuchsia存档格式是Zircon和Fuchsia使用的文件的容器。
<!-- - [FAR Spec](the-book/archive_format.md) -->
- [FAR规范（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/archive_format.md)

#### **fdio**

<!-- fdio is the Zircon IO Library.  It provides the implementation of posix-style open(), close(),
read(), write(), select(), poll(), etc, against the RemoteIO RPC protocol.  These APIs are
return-not-supported stubs in libc, and linking against libfdio overrides these stubs with
functional implementations. -->
fdio是Zircon的IO库，它针对RemoteIO的RPC协议提供了posix样式的`open()`，`close()`，`read()`，`write()`，`select()`和`poll()`函数等的实现。这些API是libc中“返回libc不支持该操作”的stub，而与libfdio的链接会用其实现将这些stub覆盖掉。
<!-- - [Source](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/fdio/) -->
- [源码](https://github.com/fuchsia-mirror/zircon/tree/master/system/ulib/fdio)

#### **FIDL**
<!-- 
The Fuchsia Interface Definition Language (FIDL) is a language for defining protocols
for use over [Channels](#channel). FIDL is programming language agnostic and has
bindings for many popular languages, including C, C++, Dart, Go, and Rust. This
approach lets system components written in a variety of languages interact seamlessly. -->

Fuchsia接口定义语言(FIDL)是一种用于定义在[Channel](#channel)上使用的协议的语言。FIDL是编程语言无关的，并且具有许多流行语言的绑定，包括C，C ++，Dart，Go和Rust，这种方法可以使用各种语言编写系统组件进行无缝交互。

#### **Flutter**

<!-- [Flutter](https://flutter.io/) is a functional-reactive user interface framework
optimized for Fuchsia and is used by many system components. Flutter also runs on
a variety of other platform, including Android and iOS. Fuchsia itself does not
require you to use any particular language or user interface framework. -->
[Flutter](https://flutter.io/)是针对Fuchsia优化的功能反应用户界面框架，并已经被许多系统组件使用。Flutter还可以运行在各种其他平台上，包括Android和iOS。值得注意的是，Fuchsia本身不要求你使用任何特定语言或用户界面框架。

#### **Fuchsia Package**

<!-- A Fuchsia Package is a unit of software distribution. It is a collection of
files, such as: manifests, metadata, zero or more executables (e.g.
[Components](#component)), and assets. -->
Fuchsia软件包(Fuchsia Package)是软件分发的单元，它是由一组文件构成，包括清单文件、元数据、零个或多个可执行文件（如[组件](#component)）和资源等。

#### **FVM**

<!-- Fuchsia Volume Manager is a partition manager providing dynamically allocated
groups of blocks known as slices into a virtual block address space. The
FVM partitions provide a block interface enabling filesystems to interact
with it in a manner largely consistent with a regular block device.
- [Filesystems](the-book/filesystems.md) -->

Fuchsia Volume Manager (FVM)是一个分区管理器，它将动态分配的块（称为slice）提供给虚拟块设备地址空间。FVM分区提供了一个块设备接口，使文件系统能够以与常规块设备基本一致的方式与其进行交互。
- [文件系统](the-book/filesystems.md)

#### **Garnet**
<!-- 
Garnet is one of the four layers of the Fuchsia codebase.
- [The Fuchsia layer cake](development/source_code/layers.md)
- [Source](https://fuchsia.googlesource.com/garnet/+/master) -->
Garnet是Fuchsia代码库的四个层之一。

- [Fuchsia代码的糕式层叠结构](development/source_code/layers.md)
- [源码](https://github.com/fuchsia-mirror/garnet)

#### **GN**
<!-- 
GN is a meta-build system which generates build files so that Fuchsia can be
built with [Ninja](#ninja).
GN is fast and comes with solid tools to manage and explore dependencies.
GN files, named `BUILD.gn`, are located all over the repository. -->
GN是一个可生成构建文件的元构建系统，可以利用它来使用[Ninja](#ninja)构建Fuchsia。GN速度快，并配有可靠的工具来管理和检索依赖项。名为`BUILD.gn`的GN文件位于整个代码仓库z中。
<!-- - [Language and operation](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/docs/language.md)
- [Reference](https://chromium.googlesource.com/chromium/src/tools/gn/+/HEAD/docs/reference.md)
- [Fuchsia build overview](development/build/overview.md) -->
- [语言和操作（英文）](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/docs/language.md)
- [参考（英文）](https://chromium.googlesource.com/chromium/src/tools/gn/+/HEAD/docs/reference.md)
- [Fuchsia构建概览（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/development/build/overview.md)

#### **Handle**

<!-- The "file descriptor" of the Zircon kernel.  A Handle is how a userspace process refers
to a kernel object.  They can be passed to other processes over [Channel](#Channel)s. -->
Handle（句柄）是Zircon内核的“文件描述符”，它表示用户空间进程如何引用内核对象，通过[Channel](#channel)可以将Handle传递给其他进程。
<!-- - [Handle (in Zircon Concepts Doc)](https://fuchsia.googlesource.com/zircon/+/master/docs/concepts.md) -->

- [（在Zircon概念文档中）的Handle文档](/zircon/docs/concepts.md)

#### **Hub**

<!-- The hub is a portal for introspection.  It enables tools to access detailed
structural information about realms and component instances at runtime,
such as their names, job and process ids, and published services. -->
Hub是用于内省的入口，它使工具能够在运行时访问有关领域和组件实例的详细结构信息，例如名称，job和进程ID以及已发布的服务等。
<!-- - [Hub](the-book/hub.md) -->
- [Hub（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/hub.md)

#### **Jiri**

<!-- Jiri is a tool for multi-repo development. It is used to checkout the Fuchsia
codebase. It supports various subcommands which makes it easy for developers to
manage their local checkouts. -->
Jiri是跨仓库开发工具，它用于检出(checkout)Fuchsia代码库，并具有各种子命令支持，使开发人员可以轻松管理本地的代码检出。

<!-- - [Reference](https://fuchsia.googlesource.com/jiri/+/master/README.md)
- [Sub commands](https://fuchsia.googlesource.com/jiri/+/master/README.md#main-commands-are)
- [Behaviour](https://fuchsia.googlesource.com/jiri/+/master/behaviour.md)
- [Tips and tricks](https://fuchsia.googlesource.com/jiri/+/master/howdoi.md) -->
- [参考](/jiri/README.md)
- [子命令](/jiri/README.md#主要的子命令包括)
- [行为](/jiri/behaviour.md)
- [建议和技巧](/jiri/howdoi.md)

#### **Job**

<!-- TODO(cpu): add definition -->
TODO(cpu): 增加定义

#### **Ledger**

<!-- [Ledger](https://fuchsia.googlesource.com/peridot/+/master/docs/ledger/README.md)
is a distributed storage system for Fuchsia. Applications use Ledger either
directly or through state synchronization primitives exposed by the Modular
framework that are based on Ledger under-the-hood. -->
[Ledger](/peridot/docs/ledger/README.md)是Fuchsia的分布式存储系统。应用程序使用Ledger的方式包括，直接或通过模块化框架公开的状态来同步原语，这些原语基于Ledger内部的操作。

#### **LK**

<!-- Little Kernel (LK) is the embedded kernel that formed the core of the Zircon Kernel.
LK is more microcontroller-centric and lacks support for MMUs, userspace, system calls --
features that Zircon added. -->
Little Kernel(LK)是嵌入式内核，它构成了Zircon内核的核心。LK以微控制器为目标硬件，缺乏对MMU、用户空间和系统调用的支持——而这些皆是Zircon在其基础上增加的功能。
<!-- - [LK on Github]（https://github.com/littlekernel/lk） -->
- [Github中的LK项目](https://github.com/littlekernel/lk)

#### **Maxwell**
<!-- 
Services to expose ambient and task-related context, suggestions and
infrastructure for leveraging machine intelligence. -->
Maxwell是用于客户端使用机器智能的服务，包括公开周围和任务相关的上下文、建议和基础设施等。

#### **Module**

<!-- A component with a `module` metadata file which primarily describes the
Module's data compatibility and semantic role.

Modules show UI and participate in Stories at runtime. -->
Module是具有`module`元数据文件的组件，主要描述Module的数据兼容性和语义角色。

Module在运行时显示UI并参与到Story中。
<!-- - [module metadata format](https://fuchsia.googlesource.com/peridot/+/HEAD/docs/modular/manifests/module.md) -->
- [模块元数据格式（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/modular/manifests/module.md)

#### **Mozart**

<!-- The view subsystem. Includes views, input, compositor, and GPU service. -->
Mozart是Fuchsia视图子系统，包括视图，输入，合成器和GPU服务。

#### **Musl**

<!-- Fuchsia's standard C library (libc) is based on Musl Libc. -->
Fuchsia的标准C语言库(libc)是基于Musl的。

<!-- - [Source](https://fuchsia.googlesource.com/zircon/+/master/third_party/ulib/musl/)
- [Musl Homepage](https://www.musl-libc.org/) -->
- [源码](https://github.com/fuchsia-mirror/zircon/tree/master/third_party/ulib/musl)
- [Musl主页](https://www.musl-libc.org/)

#### **Namespace**

<!-- A namespace is the composite hierarchy of files, directories, sockets, [service](#Service)s,
and other named objects which are offered to components by their
[environment](#Environment). -->
命名空间是文件、目录、套接字、[服务](#service)和其他命名对象的复合层次结构，这些对象由[环境](#environment)提供给组件。
<!-- - [Fuchsia Namespace Spec](the-book/namespaces.md) -->

- [Fuchsia命名空间规范](the-book/namespaces.md)

#### **Netstack**

<!-- TODO(tkilbourn): add definition. -->
TODO(tkilbourn): 增加定义。

#### **Ninja**

<!-- Ninja is the build system executing Fuchsia builds.
It is a small build system with a strong emphasis on speed.
Unlike other systems, Ninja files are not supposed to be manually written but
should be generated by other systems, such as [GN](#gn) in Fuchsia. -->
Ninja是具体编译Fuchsia的构建系统。它是一个追求速度的小型构建系统。与其他系统不同的是，无需手动编写Ninja文件，而由其他系统，如Fuchsia中的[GN](#gn)，生成而来。
<!-- - [Manual](https://ninja-build.org/manual.html)
- [Ninja rules in GN](https://gn.googlesource.com/gn/+/master/tools/gn/docs/reference.md#ninja_rules)
- [Fuchsia build overview](development/build/overview.md) -->
- [手册](https://ninja-build.org/manual.html)
- [GN中的Ninja规则](https://gn.googlesource.com/gn/+/master/tools/gn/docs/reference.md#ninja_rules)
- [Fuchsia构建概览（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/development/build/overview.md)

#### **Package**

<!-- Package is an overloaded term. Package may refer to a
[Fuchsia Package](#fuchsia-package) or a [GN build package](#GN). -->

Package是一个具有多重含义的术语，它可以指[Fuchsia软件包](#fuchsia-package)或[GN构建软件包](#gn)。

#### **Paver**

<!-- A tool in Zircon that installs partition images to internal storage of a device. -->
Zircon中的工具，用于将分区映像文件安装到设备的内部存储器中。
<!-- - [Guide for installing Fuchsia with paver](/development/workflows/fuchsia_paver.md). -->
- [用Paver安装Fuchsi的指南（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/development/workflows/fuchsia_paver.md).

#### **Peridot**

<!-- Peridot is one of the four layers of the Fuchsia codebase. -->
Peridot是Fuchsia代码库的四个层之一。

<!-- - [The Fuchsia layer cake](development/source_code/layers.md)
- [Source](https://fuchsia.googlesource.com/peridot/+/master) -->
- [Fuchsia代码的糕式层叠结构](development/source_code/layers.md)
- [源码](https://github.com/fuchsia-mirror/peridot)

#### **Realm**

<!-- Synonym for [environment](#environment). -->
[Environment](#environment)的同义词。

#### **RemoteIO**

<!-- RemoteIO is the Zircon RPC protocol used between fdio (open/close/read/write/ioctl)
and filesystems, device drivers, etc.  As part of [FIDL](#FIDL) v2, it will become a set
of FIDL Interfaces (File, Directory, Device, ...) allowing easier interoperability,
and more flexible asynchronous IO for clients or servers. -->

RemoteIO是在fdio（打开/关闭/读/写/ioctl）和文件系统、设备驱动程等之间交互使用的Zircon RPC协议。作为[FIDL](#fidl) v2的一部分，它将成为一组允许更容易的互操作性，以及更灵活的客户端或服务器异步IO的FIDL接口（文件、目录、设备……）。

#### **Service**

<!-- A service is an implementation of a [FIDL](#FIDL) interface. Components can offer
their creator a set of services, which the creator can either use directly or
offer to other components. -->
服务(Service)是[FIDL](#fidl)接口的实现。组件可以为其创建者提供一组服务，创建者可以直接使用这些服务，也可以将其提供给其他组件。

<!-- Services can also be obtained by interface name from a [Namespace](#namespace),
which lets the component that created the namespace pick the implementation of
the interface. Long-running services, such as [Mozart](#mozart), are typically
obtained through a [Namespace](#namespace), which lets many clients connect to a
common implementation. -->
服务也可以通过[命名空间](#namespace)的接口名称获得，这使得创建命名空间的组件可以选择接口的实现实现方式。长期运行的服务，例如[Mozart](#mozart)，通常通过[命名空间](#namespace)获得，它允许大量客户端连接到同一实现上。

#### **Story**

<!-- A user-facing logical container encapsulating human activity, satisfied by one
or more related modules. Stories allow users to organize activities in ways they
find natural, without developers having to imagine all those ways ahead of time. -->

Stroy是面向用户的逻辑容器，它封装用户的活动并由一个或多个相关模块实现。Stroy允许用户以自然的方式组织活动，而开发人员不必提前了解所有的这些方式。

#### **Story Shell**

<!-- The system responsible for the visual presentation of a story. Includes the
presenter component, plus structure and state information read from each story. -->
负责Story视觉呈现的系统，它包括演示者组件，以及负责从每个Story中读取的结构体和状态信息。

#### **Topaz**

<!-- Topaz is one of the four layers of the Fuchsia codebase. -->
Topaz是Fuchsia代码库的四个层之一。
<!-- - [The Fuchsia layer cake](development/source_code/layers.md)
- [Source](https://fuchsia.googlesource.com/topaz/+/master) -->
- [Fuchsia代码的糕式层叠结构](development/source_code/layers.md)
- [源码](https://github.com/fuchsia-mirror/topaz)

#### **User Shell**

<!-- The user-specific and replaceable set of software functionality that works in
conjunction with devices to create an environment in which people can interact
with modules. -->

User Shell是针对特定使用者的，且可替换的一组软件功能，与设备结合使用以创建用户与模块交互的环境。

#### **VDSO**

<!-- The VDSO is a Virtual Shared Library -- it is provided by the [Zircon](#Zircon) kernel
and does not appear in the filesystem or a package.  It provides the Zircon System Call
API/ABI to userspace processes in the form of an ELF library that's "always there."
In the Fuchsia SDK and [Zircon DDK](#DDK) it exists as `libzircon.so` for the purpose of
having something to pass to the linker representing the VDSO. -->

VDSO是一个虚拟共享库——它由[Zircon](#zircon)内核提供，且不会出现在文件系统或软件包中，它以“永远存在”的ELF库的形式向用户空间进程提供Zircon系统调用API/ABI。在Fuchsia的SDK和[Zircon的DDK](#ddk)中，它以`libzircon.so`存在，目的是将某些内容传递给代表VDSO的链接器。

#### **VMAR**

<!-- A Virtual Memory Address Range is a Zircon Kernel Object that controls where and how
VMOs may be mapped into the address space of a process. -->
VMAR(Virtual Memory Address Range)是Zircon内核对象，它负责VMO的位置以及如何将其映射到进程的地址空间上。
<!-- - [VMAR Overview](https://fuchsia.googlesource.com/zircon/+/master/docs/objects/vm_address_region.md) -->
- [VMAR概览](https://github.com/fuchsia-mirror/zircon/blob/master/docs/objects/vm_address_region.md)

#### **VMO**

<!-- A Virtual Memory Object is a Zircon Kernel Object that represents a collection of pages
(or the potential for pages) which may be read, written, mapped into the address space of
a process, or shared with another process by passing a Handle over a Channel. -->
VMO(Virtual Memory Object)是Zircon内核对象，它表示可以读取、写入、映射到进程的地址空间，或通过传递Channel传递的Handle进行共享的一组页面（或潜在的页面）集合。
<!-- - [VMO Overview](https://fuchsia.googlesource.com/zircon/+/master/docs/objects/vm_object.md) -->
- [VMO概览](https://github.com/fuchsia-mirror/zircon/blob/master/docs/objects/vm_object.md)

#### **Zedboot** ####

<!-- TODO(raggi): add definition. -->
TODO(raggi): 增加定义。

#### **Zircon**

<!-- Zircon is the [microkernel](https://en.wikipedia.org/wiki/Microkernel) and lowest level
userspace components (driver runtime environment, core drivers, libc, etc) at the core of
Fuchsia.  In a traditional monolithic kernel, many of the userspace components of Zircon
would be part of the kernel itself.
Zircon is also one of the four layers of the Fuchsia codebase. -->
Zircon是Fuchsia的[微内核](https://en.wikipedia.org/wiki/Microkernel)和位于Fuchsia核心位置的最低层次的用户空间组件（驱动运行时环境、核心驱动和libc等）。在传统宏内核中，Zircon的许多用户空间组件也都是内核本身的一部分。Zircon也是Fuchsia代码库的四层之一。
<!-- - [Zircon Documentation](https://fuchsia.googlesource.com/zircon/+/master/README.md)
- [Zircon Concepts](https://fuchsia.googlesource.com/zircon/+/master/docs/concepts.md)
- [The Fuchsia layer cake](development/source_code/layers.md)
- [Source](https://fuchsia.googlesource.com/zircon/+/master) -->
- [Zircon文档](/zircon/README.md)
- [Zircon概念](/zircon/docs/concepts.md)
- [Fuchsia代码的糕式层叠结构](development/source_code/layers.md)
- [源码](https://github.com/fuchsia-mirror/zircon)



#### **ZX**

<!-- ZX is an abbreviation of "Zircon" used in Zircon C APIs/ABIs (`zx_channel_create()`, `zx_handle_t`,
 `ZX_EVENT_SIGNALED`, etc) and libraries (libzx in particular). -->
 ZX是Zircon中的C语言API/ABI（包括`zx_channel_create()`，`zx_handle_t`，`ZX_EVENT_SIGNALED`等）和运行库（特别是libzx）中使用的"Zircon"的缩写。
<!--
# FIDL: Overview
-->
FIDL概述
----

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/index.md)

----
<!-- 
This document is a description of the Fuchsia Interface Definition Language
(FIDL) purpose, high-level goals, and requirements.
-->
本文档的目的是用来描述Fuchsia接口定义语言（FIDL）的高层目标与需求。
<!-- 
 ## Related Documents

*   [FIDL: Wire Format Specification]
*   [FIDL: Language Specification]
*   [FIDL: Compiler Specification]
*   [FIDL: C Language Bindings]
*   [FIDL: C++ Language Bindings]
*   [FIDL Examples]: Some small example code used during development

<!-- Reference links because these are used again below. -->
<!--
[FIDL: Wire Format Specification]: wire-format/index.md
[FIDL: Language Specification]: language.md
[FIDL: Compiler Specification]: compiler.md
[FIDL: C Language Bindings]: c-language-bindings.md
[FIDL: C++ Language Bindings]: c++-language-bindings.md
[FIDL Examples]: ../../system/host/fidl/examples
-->
## 相关文档

*   [FIDL: 传输格式规范][FIDL: Wire Format Specification]
*   [FIDL: 语言规范][FIDL: Language Specification]
*   [FIDL: 编译规范][FIDL: Compiler Specification]
*   [FIDL: 绑定C语言][FIDL: C Language Bindings]
*   [FIDL: 绑定C++语言][FIDL: C++ Language Bindings]
*   [FIDL示例][FIDL Examples]

[FIDL: Wire Format Specification]: https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/wire-format/index.md
[FIDL: Language Specification]: https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/language.md
[FIDL: Compiler Specification]: https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/compiler.md
[FIDL: C Language Bindings]: https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/c-language-bindings.md
[FIDL: C++ Language Bindings]: https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/c++-language-bindings.md
[FIDL Examples]: https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/../../system/host/fidl/examples

[TOC]

<!--
Overview
-->

# 概述
<!--
The Fuchsia Interface Definition Language (FIDL) is the language used to
describe interprocess communication (IPC) protocols used by programs running on
the Fuchsia Operating System. FIDL is supported by a toolchain (compiler) and
runtime support libraries (bindings) to help developers use IPC effectively.
-->

Fuchsia接口定义语言（FIDL）是用来描述Fuchsia操作系统中进程间通信协议（IPC）的语言。FIDL的工具链（编译   器）和运行时支持库(绑定)用于帮助开发者高效的使用IPC。

<!--
Goals
-->
# 目标

<!--
Fuchsia extensively relies on IPC since it has a microkernel architecture
wherein most functionality is implemented in user space outside of the kernel,
including privileged components such as device drivers. Consequently the IPC
mechanism must be efficient, deterministic, robust, and easy to use.
-->
由于Fuchsia是微内核操作系统，其中大部分的功能在用户空间中实现，包括设备驱动等特权组件，所以它大量地依赖IPC进行通信。因此，IPC机制在设计上必须具有高效性、确定性、稳健性和易用性。

<!--
**IPC efficiency** pertains to the computational overhead required to generate,
transfer, and consume messages between processes. IPC will be involved in all
aspects of system operation so it must be efficient. The FIDL compiler must
generate tight code without excess indirection or hidden costs. It should be at
least as good as hand-rolled code would be where it matters most.
-->
**IPC的高效性**衡量生成、传输和处理进程间消息所需的计算开销。IPC将参与所有的系统操作，所以它必须高效。FIDL的编译器必须生成紧凑的代码，没有额外的间接跳转或者隐形开销。最重要的是，它应该至少要你特定优化的代码一样好。
<!--
**IPC determinism** pertains to the ability to perform transactions within a
known resource envelope. IPC will be used extensively by critical system
services such as filesystems which serve many clients and which must perform in
predictable ways. The FIDL wire format must offer strong static guarantees such
as ensuring that structure size and layout is invariant thereby alleviating the
need for dynamic memory allocation or complex validation rules.
-->
**IPC的确定性**衡量在已知的封装资源大小的执行事务能力。IPC将被广泛的用于关键系统服务，例如，服务于许多客户端的文件系统，必须按照可预测的方式进行工作。FIDL的传输格式必须对确保结构体大小与布局的不变性提供强静态保证，从而减轻对动态内存分配或复杂验证规则的需求。

<!--
**IPC robustness** pertains to the need to consider IPC as an essential part of
the operating system's ABI. Maintaining binary stability is crucial. Mechanisms
for protocol evolution must be designed conservatively so as not to violate the
invariants of existing services and their clients, particularly when the need
for determinism is also considered. The FIDL bindings must perform effective,
lightweight, and strict validation.
-->
**IPC的鲁棒性**衡量考虑IPC作为操作系统ABI的重要组成部分的需要。保持二进制的稳定性至关重要，协议演变的机制必须谨慎设计，以便使现在的服务与他们的客户端不违反不变性，特别是在确定性的需求也被考虑其中时。FIDL的绑定也必须高效，轻量并且经过严格验证。

<!--
**IPC ease of use** pertains to the fact that IPC protocols are an essential
part of the operating system's API. It is important to provide good developer
ergonomics for accessing services via IPC. The FIDL code generator removes the
burden of writing IPC bindings by hand. Moreover, the FIDL code generator can
produce different bindings to suit the needs of different audiences and their
    .
-->
**IPC的易用性**衡量IPC协议作为操作系统API的重要组成部分，为通过IPC访问服务提供好的开发者使用方法是很重要的。FIDL的代码生成器减轻了手工编写IPC绑定代码的负担。此外，FIDL的代码生成器可以提供不同的绑定来适应不同开发者以及他们的习惯。
<!--
TODO: express goal of meeting the needs of different audiences using
appropriately tailored bindings, eg. system programming native vs. event-driven
dispatch vs. async calls, etc... say more things about FIDL as our system API,
SDK concerns, etc.
-->
TODO: 解释为满足不同受众使用合适的定制化绑定的目标是什么，例如，本地系统编程 vs.事件驱动调度 vs. 异步调用等... 以及关于更多FIDL的介绍，例如系统API，SDK的关注点等。

<!--
Requirements
-->
# 需求
<!--
# Purpose
-->
## 目的

<!--
*   Describe data structures and interfaces used by IPC protocols on Zircon.
*   Optimized for interprocess communication only; FIDL must not be persisted to
    disk or used for network transfer across device boundaries.
*   Efficiently transport messages consisting of data (bytes) and capabilities
    (handles) over Zircon channels between processes running on the same
    device.
*   Designed specifically to facilitate effective use of Zircon primitives; not
    intended for use on other platforms; not portable.
*   Offers convenient APIs for creating, sending, receiving, and consuming
    messages.
*   Perform sufficient validation to maintain protocol invariants (but no more
    than that).
-->
*   描述用于Zircon上使用的IPC协议的数据结构与接口。
*   针对特定于进程间通信的优化; FIDL不能用于磁盘上的持久化操作或者跨设备的网络传输。
*   同一设备上进程间的有效传输消息由数据(字节)与Zircon中处理通道的能力组成。
*   专为促进Zircon原语的有效使用而设计；不打算在其它平台上使用，并且不可移植。
*   为创建、发送、接收与消费消息提供方便的API。
*   执行足够的验证来维护协议不变性（并仅是如此而已）。
<!--
# Efficiency
-->

## 性能
<!--
*   Just as efficient (speed and memory) as using hand-rolled data structures
    would be.
*   Wire format uses uncompressed native datatypes with host-endianness and
    correct alignment to support in-place access of message contents.
*   No dynamic memory allocation is required to produce or to consume messages
    when their size is statically known or bounded.
*   Explicitly handle ownership with move-only semantics.
*   Data structure packing order is canonical, unambiguous, and has minimum
    padding.
*   Avoid back-patching pointers.
*   Avoid expensive validation.
*   Avoid calculations which may overflow.
*   Leverage pipelining of interface requests for asynchronous operation.
*   Structures are fixed size; variable-size data is stored out-of-line.
*   Structures are not self-described; FIDL files describe their contents.
*   No versioning of structures, but interfaces can be extended with new methods
    for protocol evolution.
-->

*   与使用手动定义数据的方式一样（在速度与内存使用上）高效。
*   在传输格式上，使用未压缩的本地主机大小端数据类型，并纠正数据对齐来支持消息内容的原地访问。
*   如果消息大小静态已知或者有界时，无需分配动态内存以产生或消费消息。
*   利用move-only语义来显式处理所有权
*   数据结构打包顺序是规范的，无歧义的，并且是最小对齐的。
*   避免反向修改指针。
*   避免高性能损耗的验证操作。
*   避免可能栈溢出的计算。
*   将接口请求流水化实现异步操作。
*   结构体固定大小；将可变大小的数据存储在外部。
*   结构体不能自描述；FIDL文件用于描述结构体的内容。
*   结构没有版本控制，但是接口可扩展新的方法来演化协议。

<!--
# Ergonomics
-->
## 人类工程学
<!--
*   Programming language bindings maintained by Fuchsia team:
    *   C, C++ (native), C++ (idiomatic), Dart, Go, Rust
*   Keeping in mind we might want to support other languages in the future, such
    as:
    *   Java, Javascript, etc.
*   The bindings and generated code are available in native or idiomatic flavors
    depending on the intended application.
*   Use compile-time code generation to optimize message serialization,
    deserialization, and validation.
*   FIDL syntax is familiar, easily accessible, and programming language
    agnostic.
*   FIDL provides a library system to simplify deployment and use by other
    developers.
*   FIDL expresses the most common data types needed for system APIs; it does
    not seek to provide a comprehensive one-to-one mapping of all types offered
    by all programming languages.
-->

*   Fuchsia团队维护的编程语言绑定:
    *   C, C++ (native), C++ (idiomatic), Dart, Go, Rust
*   同时留意我们希望去支持更多语言，例如:
    *   Java, Javascript, etc.
*   绑定与代码生成根据原定的用途适应原有的风格。
*   在编译时生成代码，优化消息序列，反序列化，并且验证。
*   FIDL语法是熟悉的，易于访问的，并且编程语言不可知。
*   FIDL提供一个库来简化其它开发者的部署与使用。
*   FIDL表述系统API所需要的最常见的数据类型；它不寻求提供支持所有编程语言一对一的全面映射。
<!--
# Implementation
-->

# 实现
<!--
*   Compiler is written in C++ to be usable by components built in Zircon.

*   Compiler is portable and can be built with a host toolchain.

*   We will not support FIDL bindings for any platform other than Fuchsia.
-->

*   编译器使用C++编写，用于Zircon中的组件使用。

*   编译器是可移植的，并可利用宿主工具链来构建它。

*   我们不会在Fuchsia以外的平台支持FIDL绑定。
<!--
## Where to Find the Code
-->

## 代码位置

- [编译器](https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/../../system/host/fidl)
- [C绑定](https://github.com/fuchsia-mirror/zircon/blob/00faaac908ed4c5a59bfab95b6831b33df6a5cb0/docs/fidl/../../system/ulib/fidl)
- [C++绑定](https://github.com/fuchsia-mirror/garnet/tree/master/public/lib/fidl/cpp)
- [Go绑定](https://github.com/fuchsia-mirror/garnet/tree/master/public/lib/fidl/go)
- [Rust绑定](https://github.com/fuchsia-mirror/garnet/tree/master/public/lib/fidl/rust)

<!--
## Constituent Parts of Specification
-->
## 规范的组成部分

<!--
### FIDL Wire Format
-->
### FIDL传输格式

<!--
The FIDL wire format specified how FIDL messages are represented in memory for
transmission over IPC.

The fidl wire format is documented [FIDL: Wire Format Specification].
-->

FIDL传输格式规范了FIDL消息是如何在内存中表示以支持IPC传输。

[FIDL传输格式的文档][FIDL: Wire Format Specification]

<!--
### FIDL Language
-->
### FIDL语言
<!--
The FIDL language is the syntax by which interfaces are described in ***.fidl**
files.

The fidl language is documented [FIDL: Language Specification].
-->

FIDL语言是**fidl**文件来描述接口的语法。

[FIDL语言的文档][FIDL: Language Specification].

<!--
 FIDL Compiler
-->
### FIDL编译器

<!--
The FIDL compiler generates code for programs to use and implement interfaces
described by the FIDL language.

The fidl compiler is documented [FIDL: Compiler Specification].
-->

FIDL编译器的功能是为使用与实现FIDL语言描述的接口的程序生成代码。

[FIDL编译器的文档][FIDL: Compiler Specification].

<!--
 FIDL Bindings
-->

### FIDL绑定

<!--
FIDL bindings are language-specific runtime support libraries and code
generators which provide APIs for manipulating FIDL data structures and
interfaces.
-->
FIDL绑定是语言特定的运行时支持库与代码生成器，以提供用于操作FIDL数据结构和接口的API。

<!--
Languages-specific topics:

*   [FIDL: C Language Bindings]
*   [FIDL: C++ Language Bindings]
-->
语言相关的主题:
*   [FIDL: C语言绑定][FIDL: C Language Bindings]
*   [FIDL: C++语言绑定][FIDL: C++ Language Bindings]

<!--
Bindings are available in various flavors depending on the language:
-->
绑定的风格根据语言而有所不同:

<!--
   Native bindings: designed for highly sensitive contexts such as device
    drivers and high-throughput servers, leverage in-place access, avoid memory
    allocation, but may require somewhat more awareness of the constraints of
    the protocol on the part of the developer.
-->
*   **本地绑定**: 为高度敏感的上下文，如设备驱动与高吞吐量的服务器而设计，充分利用原地访问，避免内存分配，但是需要开发者了解更多关于协议上的限制。

<!--
	Idiomatic bindings: designed to be more developer-friendly by copying
    data from the wire format into easier to use data types (such as heap-backed
    strings or vectors), but correspondingly somewhat less efficient as a
    result.
-->
*   **惯用绑定**: 通过将数据从传输格式拷贝到易于使用的数据类型上（例如堆上字符串或者向量），来提供更开发者友好的放好似，但是这种方式对应的效率较低。

<!--
Bindings offer several various ways of invoking interface methods depending on
the language:
-->
绑定提供了根据不同语言调用接口的多种方法:

<!--
	Send/receive: read or write messages directly to a channel, no built-in
    wait loop (C)
	Callback-based**: received messages are dispatched asynchronously as
    callbacks on an event loop (C++, Dart)
	Port-based: received messages are delivered to a port or future (Rust)
	Synchronous call**: waits for reply and return it (Go, C++ unit tests)
-->
*   **发送/接收**: 直接通过channel读写消息，无内置等待循环（C）
*   **基于回调**: 异步分配的方式接收消息，并作为事件循环的回调（C++，Dart）
*   **基于端口**: 接收的消息传递到port或future上（Rust）
*   **同步调用**: 等待应答后返回结果（Go，C++单元测试）

<!--
Bindings provide some or all of the following principal operations:
-->
绑定提供以下主要操作的部分或全部:

<!--
*   **Encode**: in-place transform native data structures into the wire format
    (coupled with validation)
*   **Decode**: in-place transform wire format data into native data structures
    (coupled with validation)
*   **Copy/Move To Idiomatic Form**: copy contents of native data structures
    into idiomatic data structures, handles are moved
*   **Copy/Move To Native Form**: copy contents of idiomatic data structures
    into native data structures, handles are moved
*   **Clone**: copy native or idiomatic data structures (that do not contain
    move-only types)
*   **Call**: invoke interface method
-->
*   **编码**: 将本地数据结构原地转换为传输格式（与验证相结合）
*   **解码**: 将传输格式原地转换为本地数据结构（与验证相结合）
*   **复制/移动到惯用形式**: 把本地数据结构的内容复制为惯用数据结构中，同时移除handle。
*   **复制/移动到本机格式**: 把惯用数据结构的内容复制为本地数据结构中，同时移除handle。
*   **克隆**: 复制本地或惯用数据结构 (不包含只可移动的类型)
*   **调用**: 调用接口方法

<!--
## Workflow
-->

# 工作流

<!--
This section describes the workflow of authors, publishers, and consumers of IPC
protocols described using FIDL.
-->
本章节介绍了使用FIDL描述IPC协议的作者，发布者，使用者的工作流。

<!--
# Authoring FIDL
-->


## 创作 FIDL

<!--
The author of a FIDL based protocol creates one or more ***.fidl files** to
describe their data structures and interfaces.
-->
基于FIDL协议的作者创建一个或多个**fidl文件**来描述他们的数据结构与接口。

<!--
FIDL files are grouped into one or more **FIDL libraries** by the author. Each
library represents a group of logically related functionality with a unique
library name. FIDL files within the same library implicitly have access to all
other declarations within the same library. The order of declarations within the
FIDL files that make up a library is not significant.
-->
作者将FIDL文件分组为一个或多个**FIDL库**。每个库表示一组逻辑相关的功能，并具有唯一命名。在同一个库中的FIDL文件可以访问库中所有其它的声明，并且FIDL文件中的声明顺序并不重要。

<!--
FIDL files of one library can access declarations within another FIDL library by
**importing** the other FIDL module. Importing other FIDL libraries makes their
symbols available for use thereby enabling the construction of protocols derived
from them. Imported symbols must be qualified by the library name or by an alias
to prevent namespace collisions.
-->
一个FIDL库中的FIDL文件通过**importing**声明可以访问另一个库中的模块。导入其它库使得它们的符号可供使用，从而可以构建它们的派生协议。导入的符号必须通过库名或别名来限定，以防止命名空间冲突。

<!--
# Publishing FIDL
-->

## 发布 FIDL

<!--
The publisher of a FIDL based protocol is responsible for making FIDL libraries
available to consumers. For example, the author may disseminate FIDL libraries in
a public source repository or distribute them as part of an SDK.
-->
基于FIDL协议的发布者负责向使用者提供FIDL库。例如，作者可以在公共源仓库中分发FIDL库，或者将他们以SDK中的一部分的方式进行分发。

<!--
Consumers need only point the FIDL compiler at the directory which contains the
FIDL files for a library (and its dependencies) to generate code for that
library. The precise details for how this is done will generally be addressed by
the consumer's build system.
-->
使用者只需向FIDL编译器指定包含FIDL库文件的目录（以及它们的依赖项）。这项工作的具体细节通常将由使用者的构建系统来解决。

<!--
# Consuming FIDL
-->
## 使用FIDL

<!--
The consumer of a FIDL based protocol uses the FIDL compiler to generate code
suitable for use with their language runtime specific bindings. For certain
language runtimes, the consumer may have a choice of a few different flavors of
generated code all of which are interoperable at the wire format level but
perhaps not at the source level.
-->
基于FIDL的协议使用者利用FIDL编译器生成适用于他们语言运行时特定绑定的代码。对于某些语言运行时，使用者可以选择几种不同风格来生成代码，他们可以传输格式上互操作，但可能在源码级别上可能不行。

<!--
In the Fuchsia world build environment, generating code from FIDL libraries will
be done automatically for all relevant languages by individual FIDL build
targets for each library.
-->
在Fuchsia的构建环境中，从FIDL库生成代码将自动为每个相关的语言通过每个FIDL构建目标自动完成。

<!--
In the Fuchsia SDK environment, generating code from FIDL libraries will be done
as part of compiling the applications which use them.
-->
在Fuchsia的SDK环境中，从FIDL库中生成代码的过程将作为编译使用他们的应用程序的构建过程一部分。
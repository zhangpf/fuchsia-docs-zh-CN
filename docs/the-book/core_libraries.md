<!-- Fuchsia Core Libraries -->
Fuchsia核心库
======================
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/03f968808eb88e38a741804ff25c07eb2d6935ae/the-book/core_libraries.md)

---


<!-- This document describes the core libraries in the Fuchsia system, starting from
the bottom of the dependency chain. -->
本文档从依赖关系链自底向上的顺序，描述了Fuchsia系统的核心库。

<!-- # Zircon libraries -->
# 位于Zircon的系统库

## libzircon

<!-- 
This library defines the Zircon system ABI.

TODO(kulakowski) Talk about how this is not quite the kernel
syscall interface, since the VDSO abstracts that. -->

libzircon定义了Zircon系统的ABI。

TODO：由于后者是由VDSO进行抽象，阐述libzircon跟zircon内核系统调用接口的区别

## libzx
<!-- 
libzircon defines C types and function calls acting on those
objects. libzx is a light C++ wrapper around those. It adds type
safety beyond `zx_handle_t`, so that every kernel object type has a
corresponding C++ type, and adds ownership semantics to those
handles. It otherwise takes no opinions around naming or policy. -->

libzircon定义了作用于内核对象上的C语言类型和函数调用，而libzx是关于这些对象的C++封装。除了句柄类型`zx_handle_t`之外，它还增加了类型安全，因此所有的内核对象都有对应的C++类型，并且对这些句柄增加了所有权（ownership）语义。除此之外，它不在命名或策略方面增加额外的内容。

<!-- For more information about libzx, see
[its documentation](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/zx/README.md). -->

关于libzx更多的信息，请查看[其文档（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/system/ulib/zx/README.md)。

## FBL

<!-- Much of Zircon is written in C++, both in kernel and in
userspace. Linking against the C++ standard library is not especially
well suited to this environment (it is too easy to allocate, throw
exceptions, etc., and the library itself is large). There are a number
of useful constructs in the standard libary that we would wish to use,
like type traits and unique pointers. However, C++ standard libraries
are not really to be consumed piecemeal like this. So we built a
library which provides similar constructs named fbl. This library
also includes constructs not present in the standard library but which
are useful library code for kernel and device driver environments (for
instance, slab allocation). -->

包括内核和用户空间在内的相当部分Zircon代码，是由C++语言写成的。而对于内核这样的环境，链接C++标准库并不十分合适（因为太容易分配不可控的内存空间，并会抛出异常等，而且这些库本身体积也较大）。另一方面，标准库中又有一些我们想使用的，对我们有帮助的类型，如类型trait和unique指针等。但我们无法零散地只使用C++标准库这一部分的代码，因此我们构建了名为fbl的库，用于提供相似的功能函数。它同时也包含了不在标准库中但对内核和设备驱动环境有用的功能代码（如slab分配器等）。

<!-- For more information about FBL,
[read its overview](https://fuchsia.googlesource.com/zircon/+/master/docs/cxx.md#fbl). -->
关于FBL更多的信息，请查看[其文档（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/cxx.md#fbl)。

# FXL系统库

<!-- 
FXL is a platform-independent library containing basic C++ building blocks, such
as logging and reference counting. FXL depends on the C++ standard library but
not on any Zircon- or Fuchsia-specific libraries. We build FXL both for target
(Fuchsia) and for host (Linux, Mac) systems. -->

FXL是包含C++基础构建模块的平台无关库，例如日志logging和引用计数等。FXL依赖于C++标准库，却不依赖于任何与Zircon或Fuchsia有关的库。我们同时在目标系统（Fuchsia）和主机系统（Linux和Mac）上构建了FXL。

<!-- 
Generally speaking, we try to use the C++ standard library for basic building
blocks, but in some cases the C++ standard library either doesn't have something
we need (e.g., a featureful logging system) or has a version of what we need
doesn't meet our requirements (e.g., `std::shared_ptr` is twice as large as
`fxl::RefPtr`). -->

一般来讲，我们试图使用C++标准库来进行基础模块构建，但是在某些情况下，C++标准库没有我们想要的功能（例如，功能强大的日志系统）或具有所需的功能，但版本去不一致（例如，`std::shared_ptr`是`fxl::RefPtr`的两倍大小）。



# FSL系统库

<!-- FSL is a Zircon-specific library containing high-level C++ concepts for working
with the Zircon system calls. For example, FSL provides an `fsl::MessageLoop`
abstraction on top of Zircon's underlying waiting primitives. FSL also contains
helpers for working with Zircon primitives asynchronously that build upon
`fsl::MessageLoop` (e.g., for draining a socket asynchronously). -->

FSL是Zircon下特定的，包含高层C++概念并与Zircon系统调用协作的系统库。例如，FSL提供了在Zircon底层waiting等待原语之上的，名为`fsl::MessageLoop`的抽象。同时，FSL还包含了构建于`fsl::MessageLoop`基础之上的，和Zircon原语异步协作的帮助代码（如异步排空socket队列等）。

<!-- FSL depends on FXL and implements some interfaces defined in FXL, such as
`fxl::TaskRunner`. FSL also depends on `libfidl` and implements FIDL's
asynchronous waiter mechanism using `fsl::MessageLoop`. -->

FSL依赖于FXL，并且实现了FXL中定义的一些接口，如`fxl::TaskRunner`。FSL还依赖于`libfidl`，利用`fsl::MessageLoop`实现FIDL的异步等待机制。
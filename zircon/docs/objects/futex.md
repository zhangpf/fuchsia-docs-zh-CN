# Futex
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/objects/futex.md)

---
<!-- ## NAME -->
## 名称

<!-- futex - A primitive for creating userspace synchronization tools. -->
futex —— 用于创建用户空间的同步原语。

<!-- ## SYNOPSIS -->
## 概要

<!-- A **futex** is a Fast Userspace muTEX. It is a low level
synchronization primitive which is a building block for higher level
APIs such as `pthread_mutex_t` and `pthread_cond_t`. -->
**futex**是用户空间的快速muTEX。它是一个为更高级API（例如`pthread_mutex_t`和`pthread_cond_t`）提供构建模块的低层次的同步原语，。

<!-- Futexes are designed to not enter the kernel or allocate kernel
resources in the uncontested case. -->
Futex旨在不进入内核或在无竞争的情况下分配内核资源。

<!-- ## DESCRIPTION -->
## 描述

<!-- The zircon futex implementation currently supports three operations: -->

Zircon中的futex实现目前支持三种操作：

```C
    zx_status_t zx_futex_wait(const zx_futex_t* value_ptr, int current_value,
                              zx_time_t deadline);
    zx_status_t zx_futex_wake(const zx_futex_t* value_ptr, uint32_t wake_count);
    zx_status_t zx_futex_requeue(const zx_futex_t* value_ptr, uint32_t wake_count,
                                 int current_value, const zx_futex_t* requeue_ptr,
                                 uint32_t requeue_count);
```

<!-- All of these share a `value_ptr` parameter, which is the virtual
address of an aligned userspace integer. This virtual address is the
information used in kernel to track what futex given threads are
waiting on. The kernel does not currently modify the value of
`*value_ptr` (but see below for future operations which might do
so). It is up to userspace code to correctly atomically modify this
value across threads in order to build mutexes and so on. -->
所有这些系统调用都共享一个`value_ptr`参数，该参数是对齐后的用户空间整型的虚拟地址。此虚拟地址是内核中用于跟踪给定线程正在等待的futex的信息。当前，内核不修改`*value_ptr`的值（但是对于将来的操作可能会这样做，请参见下文）。用户空间代码可以正确地跨线程原子修改这个值，以便构建mutex等等。

<!-- See the [futex_wait](../syscalls/futex_wait.md),
[futex_wake](../syscalls/futex_wake.md), and
[futex_requeue](../syscalls/futex_requeue.md) man pages for more details. -->
有关futex的更多详细信息，请参见[futex_wait](../syscalls/futex_wait.md)，[futex_wake](../syscalls/futex_wake.md)和[futex_requeue](../syscalls/futex_requeue.md)手册页。

<!-- ### Differences from Linux futexes -->
### 与Linux中futex的区别
<!-- Note that all of the zircon futex operations key off of the virtual
address of an userspace pointer. This differs from the Linux
implementation, which distinguishes private futex operations (which
correspond to our in-process-only ones) from ones shared across
address spaces. -->
请注意，所有Zircon的futex操作都会关闭用户空间指针的虚拟地址。这与Linux实现有所不同，后者将私有futex操作（对应于进程内的操作）与跨地址空间共享操作区分开来。

<!-- As noted above, all of our futex operations leave the value of the
futex unmodified from the kernel. Other potential operations, such as
Linux's `FUTEX_WAKE_OP`, requires atomic manipulation of the value
from the kernel, which our current implementation does not require. -->
如上所述，Zircon所有futex操作都不会在内核中修改futex的值。而其他潜在的操作，例如Linux的`FUTEX_WAKE_OP`，需要在内核中对值进行原子操作，这对于我们当前的实现来讲是不需要的。

<!-- ### Papers about futexes -->
### 关于futex的论文
- [Fuss, Futexes and Furwocks: Fast Userlevel Locking in Linux](https://www.kernel.org/doc/ols/2002/ols2002-pages-479-495.pdf), Hubertus Franke and Rusty Russell

    <!-- This is the original white paper describing the Linux futex. It
    documents the history and design of the original implementation,
    prior (failed) attempts at creating a fast userspace
    synchronization primitive, and performance measurements. -->
    描述Linux futex的原始白皮书，它记录了futex最原始实现的历史和设计，和创建快速用户空间同步原语的先前（失败）尝试，以及关于性能的测量。
- [Futexes Are Tricky](https://www.akkadia.org/drepper/futex.pdf), Ulrich Drepper

    <!-- This paper describes some gotchas and implementation details of
    futexes in Linux. It discusses the kernel implementation, and goes
    into more detail about correct and efficient userspace
    implementations of mutexes, condition variables, and so on. -->
    本文介绍了Linux中futex的一些问题和实现细节，以及讨论了在内核中的实现。并详细介绍了mutex，条件变量(condition variables)等在用户空间的正确高效的实现。
- [Mutexes and Condition Variables using Futexes](http://locklessinc.com/articles/mutex_cv_futex/)

    <!-- Further commentary on "Futexes are tricky", outlining a simple
    implementation that avoids the need for `FUTEX_CMP_REQUEUE` -->
    关于"Futexes are tricky"的进一步评论，概述了一个避免'FUTEX_CMP_REQUEUE'操作的简单futex实现。
- [Locking in WebKit](https://webkit.org/blog/6161/locking-in-webkit/), Filip Pizlo

    <!-- An in-depth tour of the locking primitives in WebKit, complete with
    benchmarks and analysis. Contains a detailed explanation of the "parking
    lot" concept, which allows very compact representation of userspace
    mutexes. -->
    深入浏览了WebKit中的加锁原语，并完成基准测试和分析，以及包含了"parking
    lot"概念的详细说明。"parking lot"允许非常紧凑地表示用户空间mutex。

<!-- ## SYSCALLS -->
## 系统调用

+ [futex_wait](../syscalls/futex_wait.md)
+ [futex_wake](../syscalls/futex_wake.md)
+ [futex_requeue](../syscalls/futex_requeue.md)

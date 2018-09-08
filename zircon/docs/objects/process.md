<!-- # Process -->
# Process（进程）
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/c90f6a56c60f0484be9f610096fb8d58edfef424/docs/objects/process.md)

---
<!-- ## NAME -->
## 名称

<!-- process - Process abstraction -->
process —— 关于进程的抽象

<!-- ## SYNOPSIS -->
## 概要

<!-- A zircon process is an instance of a program in the traditional
sense: a set of instructions which will be executed by one or more
threads, along with a collection of resources. -->
Zircon的进程是传统意义上程序的实例：由一个或多个线程执行的一组指令以及相关的资源集合组成。

<!-- ## DESCRIPTION -->
## 描述

<!-- The process object is a container of the following resources: -->
进程对象是以下资源的容器集合：

<!-- + [Handles](../handles.md)
+ [Virtual Memory Address Regions](vm_address_region.md)
+ [Threads](thread.md) -->
+ [句柄（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/handles.md)
+ [VMAR（虚拟内存地址区域）](vm_address_region.md)
+ [线程](thread.md)

<!-- In general, it is associated with code which it is executing until it is
forcefully terminated or the program exits. -->
通常，它与正在执行的代码相关联，直到强制终止或程序退出为止。

<!-- Processes are owned by [jobs](job.md) and allow an application that is
composed by more than one process to be treated as a single entity, from the
perspective of resource and permission limits, as well as lifetime control. -->
进程由[作业](job.md)所拥有。
并且从资源和权限限制以及生命周期的控制的角度上看，由多个进程组成的应用程序被视为单个实体。

<!-- ### Lifetime -->
### 生命周期

<!-- A process is created via `zx_process_create()` and its execution begins with
`zx_process_start()`. -->
进程通过`zx_process_create()`函数创建，它的执行以`zx_process_start()`开始。

<!-- The process stops execution when:
+ the last thread is terminated or exits
+ the process calls  ``zx_process_exit()``
+ the parent job terminates the process
+ the parent job is destroyed -->
进程在以下情况会停止执行：
+ 进程的最后一个线程被终止或退出
+ 进程调用``zx_process_exit()``
+ 它的父进程终止该进程
+ 父进程被销毁

<!-- The call to `zx_process_start()` cannot be issued twice. New threads cannot
be added to a process that was started and then its last thread has exited. -->
不能调用`zx_process_start()`函数两次。
不能将新线程添加到已启动的且最后一个线程已退出的进程中。

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [process_create](../syscalls/process_create.md) - create a new process within a job
+ [process_read_memory](../syscalls/process_read_memory.md) - read from a process's address space
+ [process_start](../syscalls/process_start.md) - cause a new process to start executing
+ [process_write_memory](../syscalls/process_write_memory.md) - write to a process's address space
+ [process_exit](../syscalls/process_exit.md) - exit the current process -->
+ [process_create](../syscalls/process_create.md) —— 在作业中创建新的进程
+ [process_read_memory](../syscalls/process_read_memory.md) —— 从进程的地址空间读取
+ [process_start](../syscalls/process_start.md) —— 触发新进程的执行
+ [process_write_memory](../syscalls/process_write_memory.md) —— 写入进程的地址空间
+ [process_exit](../syscalls/process_exit.md) —— 退出当前进程
<br>

<!-- + [job_create](../syscalls/job_create.md) - create a new job within a parent job -->
+ [job_create](../syscalls/job_create.md) —— 在父作业中创建新的子作业
<br>

<!-- + [vmar_map](../syscalls/vmar_map.md) - Map memory into an address space range
+ [vmar_protect](../syscalls/vmar_protect.md) - Change permissions on an address space range
+ [vmar_unmap](../syscalls/vmar_unmap.md) - Unmap memory from an address space range -->
+ [vmar_map](../syscalls/vmar_map.md) —— 将内存映射到某个VMAR上
+ [vmar_protect](../syscalls/vmar_protect.md) —— 修改VMAR的权限
+ [vmar_unmap](../syscalls/vmar_unmap.md) —— 从VMAR上取消内存映射

<!-- # Zircon Kernel objects -->
# Zircon内核对象
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/599a37ad75ad6a3e7963a808e9f05c8fd6865749/docs/objects.md)

---
<!-- [TOC] -->
- [应用程序可用的内核对象](#应用程序可用的内核对象)
    - [IPC](#ipc)
    - [任务](#任务)
    - [信号](#信号)
    - [内存和地址空间](#内存和地址空间)
    - [等待原语](#等待原语)
- [驱动程序可用的内核对象](#驱动程序可用的内核对象)
- [内核对象和LK](#内核对象和lk)
- [内核对象生命周期](#内核对象生命周期)
- [Dispatcher](#dispatcher)
- [内核对象安全](#内核对象安全)
- [另见](#另见)
  
<!-- Zircon is a object-based kernel. User mode code almost exclusively interacts
with OS resources via object handles. A handle can be thought of as an active
session with a specific OS subsystem scoped to a particular resource. -->
Zircon是一个基于对象的内核。 
用户空间代码几乎完全通过对象句柄与OS资源交互。 
句柄可被认为是具有作用于特定资源的特定OS子系统的活动会话。

<!-- Zircon actively manages the following resources: -->
Zircon积极地管理以下资源：

<!-- + processor time
+ memory and address spaces
+ device-io memory
+ interrupts
+ signaling and waiting -->
+ 处理器时间
+ 内存和地址空间
+ 设备IO内存
+ 中断
+ 信号和等待

<!-- ## Kernel objects for applications -->
## 应用程序可用的内核对象

### IPC
<!-- + [Channel](objects/channel.md) -->
+ [通道](objects/channel.md)
+ [Socket](objects/socket.md)
+ [FIFO](objects/fifo.md)

<!-- ### Tasks -->
### 任务
<!-- + [Process](objects/process.md)
+ [Thread](objects/thread.md)
+ [Job](objects/job.md)
+ [Task](objects/task.md) -->
+ [进程](objects/process.md)
+ [线程](objects/thread.md)
+ [作业](objects/job.md)
+ [任务](objects/task.md)

<!-- ### Signaling -->
### 信号

<!-- + [Event](objects/event.md)
+ [Event Pair](objects/eventpair.md)
+ [Futex](objects/futex.md) -->
+ [事件](objects/event.md)
+ [事件对](objects/eventpair.md)
+ [Futex](objects/futex.md)


<!-- ### Memory and address space -->
### 内存和地址空间

<!-- + [Virtual Memory Object](objects/vm_object.md)
+ [Virtual Memory Address Region](objects/vm_address_region.md)
+ [bus_transaction_initiator](objects/bus_transaction_initiator.md) -->
+ [虚拟内存对象(VMO)](objects/vm_object.md)
+ [虚拟内存地址区域(VMAR)](objects/vm_address_region.md)
+ [总线事务启动器(BTI)](objects/bus_transaction_initiator.md)

<!-- ### Waiting -->
### 等待原语
<!-- + [Port](objects/port.md) -->
+ [端口](objects/port.md) 

<!-- ## Kernel objects for drivers -->
## 驱动程序可用的内核对象

<!-- + [Interrupts](objects/interrupts.md)
+ [Resource](objects/resource.md)
+ [Log](objects/log.md) -->
+ [中断](objects/interrupts.md)
+ [资源](objects/resource.md)
+ [日志](objects/log.md)

<!-- ## Kernel Object and LK -->
## 内核对象和LK
<!-- Some kernel objects wrap one or more LK-level constructs. For example the
Thread object wraps one `thread_t`. However the Channel does not wrap
any LK-level objects. -->
一些内核对象封装一个或多个LK层的结构，例如，Thread对象封装了`thread_t`。
但`Channel`未封装任何LK级别的对象。

<!-- ## Kernel object lifetime -->
## 内核对象生命周期

<!-- Kernel objects are ref-counted. Most kernel objects are born during a syscall
and are held alive at refcount = 1 by the handle which binds the handle value
given as the output of the syscall. The handle object is held alive as long it
is attached to a handle table. Handles are detached from the handle table
closing them (for example via `sys_close()`) which decrements the refcount of
the kernel object. Usually, when the last handle is closed the kernel object
refcount will reach 0 which causes the destructor to be run. -->
内核对象采用引用计数的方式追踪。 
大多数内核对象在系统调用期间生成，并通过作为系统调用输出值的句柄将refcount保持在大小为1。 
只要句柄对象依然挂载到句柄表上，它就会保持活跃状态。 
如果句柄与关闭它们的句柄表相分离（例如，通过`sys_close()`调用），这将会同时减少内核对象的引用次数。
通常，当最后一个句柄关闭时，内核对象引用计数将减至0，并触发析构函数的运行。

<!-- The refcount increases both when new handles (referring to the object) are
created and when a direct pointer reference (by some kernel code) is acquired;
therefore a kernel object lifetime might be longer than the lifetime of the
process that created it. -->
当创建（引用到对象）的新句柄和（通过某些内核代码）获取直接指针引用时，引用值都会增加。
因此，内核对象的生命周期可能比创建它的进程的生命周期更长。

<!-- ## Dispatchers -->
## Dispatcher

<!-- A kernel object is implemented as a C++ class that derives from `Dispatcher`
and that overrides the methods it implements. Thus, for example, the code
of the Thread object is found in `ThreadDispatcher`. There is plenty of
code that only cares about kernel objects in the generic sense, in that case
the name you'll see is `fbl::RefPtr<Dispatcher>`. -->
内核对象以C++的类方式实现，它们派生自`Dispatcher`并覆盖其已实现的方法。
例如，`Thread`对象的代码可以在`ThreadDispatcher`中找到。 
有很多代码只关注一般意义上的内核对象，在这种情况下，你看到的名称是`fbl::RefPtr<Dispatcher>`。

<!-- ## Kernel Object security -->
## 内核对象安全

<!-- In principle, kernel objects do not have an intrinsic notion of security and
do not do authorization checks; security rights are held by each handle. A
single process can have two different handles to the same object with
different rights. -->
原则上，内核对象没有内在的安全概念，也不进行授权检查，安全权限由每个句柄进行控制。
单个进程可以提供两个不同权限的句柄，它们指向同一对象。

<!-- ## See Also -->
## 另见

<!-- [Handles](handles.md) -->
[句柄](handles.md)
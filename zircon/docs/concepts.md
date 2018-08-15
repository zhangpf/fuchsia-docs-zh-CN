<!---
# Zircon Kernel Concepts
--->
# Zircon内核概念
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/concepts.md)

---
<!---
## Introduction
--->
## 介绍

<!---
The kernel manages a number of different types of Objects. Those which are
accessible directly via system calls are C++ classes which implement the
Dispatcher interface. These are implemented in
[kernel/object](../kernel/object). Many are self-contained higher-level Objects.
Some wrap lower-level lk primitives.
--->
Zircon内核管理许多不同类型的对象。本质上讲，这些对象是能够通过系统调用直接访问，并实现了Dispatcher接口的C++类。对象的实现在[kernel/object（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/../kernel/object)目录下，它们中许多是自依赖的高层对象，而另外一些是对底层lk原始操作的封装。

<!---
## [System Calls](syscalls.md)
--->
## [系统调用](syscalls.md)

<!---
Userspace code interacts with kernel objects via system calls, and almost exclusively
via Handles.  In userspace, a Handle is represented as 32bit integer
(type zx_handle_t).  When syscalls are executed, the kernel checks that Handle
parameters refer to an actual handle that exists within the calling process's handle
table.  The kernel further checks that the Handle is of the correct type (passing
a Thread Handle to a syscall requiring an event handle will result in an error),
and that the Handle has the required Rights for the requested operation.

System calls fall into three broad categories, from an access standpoint:
--->
用户空间代码通过系统调用与内核对象进行交互，以及几乎只能通过句柄对象（`Handle`）加以访问。在用户空间中，`Handle`通过32位整型（`zx_handle_t`类型)来表示。
当系统调用被执行时，内核检查`Handle`变量指向的实际句柄是否存在于调用进程的句柄列表中。然后内核进一步检查该`Handle`是否具有正确的类型（例如传递一个Thread Handle到需要事件信号的系统调用会导致错误发生），以及`Handle`是否对请求的操作具有相应的权限（`Right`）。

从访问的角度上看，系统调动可以被细分为如下三大类：

<!---
1. Calls which have no limitations, of which there are only a very few, for
example [*zx_clock_get()*](syscalls/clock_get.md)
and [*zx_nanosleep()*](syscalls/nanosleep.md) may be called by any thread.
2. Calls which take a Handle as the first parameter, denoting the Object they act upon,
which are the vast majority, for example [*zx_channel_write()*](syscalls/channel_write.md)
and [*zx_port_queue()*](syscalls/port_queue.md).
3. Calls which create new Objects but do not take a Handle, such as
[*zx_event_create()*](syscalls/event_create.md) and
[*zx_channel_create()*](syscalls/channel_create.md).  Access to these (and limitations
upon them) is controlled by the Job in which the calling Process is contained.
--->

1. 没有任何限制的调用。只有少部分调用是属于这一类，如[*zx_clock_get()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/clock_get.md)和[*zx_nanosleep()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/nanosleep.md)可以被任意线程调用。
2. 以`Handle`作为首参数的调用，用以表示它们所操作的对象。绝大多数的调用都是这一类，例如[*zx_channel_write()*](syscalls/channel_write.md)和[*zx_port_queue()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_queue.md)。
3. 创建新对象的系统调用，不需要以`Handle`作为参数。例如[*zx_event_create()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/event_create.md)和[*zx_channel_create()*](syscalls/channel_create.md)。这一类调用的访问是受调用`Process`进程所在的`Job`所控制的（这同时也是它们的限制）。

<!---
System calls are provided by libzircon.so, which is a "virtual" shared
library that the Zircon kernel provides to userspace, better known as the
[*virtual Dynamic Shared Object* or vDSO](vdso.md).
They are C ELF ABI functions of the form *zx_noun_verb()* or
*zx_noun_verb_direct-object()*.
--->
系统调用由libzircon.so所提供。libzircon.so是由Zircon内核提供给用户空间的”虚拟“共享库，更有名的说法是[*虚拟动态共享库（ virtual Dynamic Shared Object）*或者简称为vDSO（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/vdso.md)，是由一系列以`zx_noun_verb()`或
`zx_noun_verb_direct-object()`为名的ELF ABI C函数所组成。

<!---
The system calls are defined by [syscalls.abigen](../system/public/zircon/syscalls.abigen)
and processed by the [abigen](../system/host/abigen/) tool into include files and glue
code in libzircon and the kernel's libsyscalls.
--->
系统调用由[syscalls.abigen（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/../system/public/zircon/syscalls.abigen)所定义，并被[abigen（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/../system/host/abigen/)工具所处理。`abigen`将文件和胶水代码一起添加到`libzircon`中和内核的`libsyscalls`中。

<!---
## [Handles](handles.md) and [Rights](rights.md)
--->
## 句柄（[Handle（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/handles.md)）和权限（[Right（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/rights.md)）
<!---
Objects may have multiple Handles (in one or more Processes) that refer to them.
--->
每个`Object`可以（在一个或多个进程中）有多个不同的`Handle`指向它们。

<!---
For almost all Objects, when the last open Handle that refers to an Object is closed,
the Object is either destroyed, or put into a final state that may not be undone.
--->
对于所有的`Object`，当最后指向它们的`Handle`关闭时，该`Object`同时也将被销毁，或者放置到结束状态（*译者注：类似于Java中的finalize*）下，并且这样的操作不可回滚。

<!---
Handles may be moved from one Process to another by writing them into a Channel
(using [*zx_channel_write()*](syscalls/channel_write.md)), or by using
[*zx_process_start()*](syscalls/process_start.md) to pass a Handle as the argument
of the first thread in a new Process.
--->
可以通过向`Channel`写入`Handle`的方式，将`Handle`从一个`Process`移动到另外一个`Process`（使用 [*zx_channel_write()*](syscalls/channel_write.md)函数），或通过使用[*zx_process_start()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/process_start.md)函数，作为新`Process`的第一个线程的启动参数的形式传递一个`Handle`。

<!---

The actions which may be taken on a Handle or the Object it refers to are governed
by the Rights associated with that Handle.  Two Handles that refer to the same Object
may have different Rights.
--->
在`Handle`或者它所指向的`Object`上的操作受到`Handle`相关联的权限（`Right`）所控制，并且两个指向同一`Object`的`Handle`可以具有不同的`Right`。

<!---

The [*zx_handle_duplicate()*](syscalls/handle_duplicate.md) and
[*zx_handle_replace()*](syscalls/handle_replace.md) system calls may be used to
obtain additional Handles referring to the same Object as the Handle passed in,
optionally with reduced Rights.  The [*zx_handle_close()*](syscalls/handle_close.md)
system call closes a Handle, releasing the Object it refers to, if that Handle is
the last one for that Object.
--->
[*zx_handle_duplicate()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/handle_duplicate.md)和[*zx_handle_replace()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/handle_replace.md)调用可被用于获取指向`Object`的`Handle`参数的额外副本，并以缩小的权限作为可选项。 [*zx_handle_close()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/handle_close.md)用于关闭`Handle`，如果它是所指向的`Object`的最后一个句柄，也将同时释放这个`Object`。

<!---
## Kernel Object IDs
--->
## 内核对象ID

<!---
Every object in the kernel has a "kernel object id" or "koid" for short.
It is a 64 bit unsigned integer that can be used to identify the object
and is unique for the lifetime of the running system.
This means in particular that koids are never reused.
--->
在内核中的每个对象都具有"内核对象id"或简称为“koid"。它们是64位无符号整型，用以唯一定位这些对象，并且在系统生命周期内是唯一的，这尤其意味着koid将不会被重用。

<!---
There are two special koid values:

*ZX_KOID_INVALID* Has the value zero and is used as a "null" sentinel.

*ZX_KOID_KERNEL* There is only one kernel, and it has its own koid.
--->
有两种特别的koid值：

*ZX_KOID_INVALID*的值为0，可用做表示”null“的哨兵值。

*ZX_KOID_KERNEL*只有一个内核，并且具有它自己的koid（*译者注：原文为There is only one kernel, and it has its own koid.*）。

<!---
## Running Code: Jobs, Processes, and Threads.
--->
## 运行代码：任务（Job），进程（Process）和线程（Thread）

<!---
Threads represent threads of execution (CPU registers, stack, etc) within an
address space which is owned by the Process in which they exist.  Processes are
owned by Jobs, which define various resource limitations.  Jobs are owned by
parent Jobs, all the way up to the Root Job which was created by the kernel at
boot and passed to [`userboot`, the first userspace Process to begin execution](userboot.md).
--->
`Thread`表示在拥有它们的`Process`的地址空间中线程的执行（CPU寄存器，运行栈等）。`Process`被`Job`所拥有，而后者定义了各种可用资源的上限。`Job`又被父`Job`所拥有，并一直可以追溯到根任务（`Root Job`）。根任务于内核在启动时所创建，并被传递到[第一个被执行的用户进程`userboot`（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/userboot.md)中。

<!---
Without a Job Handle, it is not possible for a Thread within a Process to create another
Process or another Job.
--->
离开了指向`Job`的`Handle`，`Process`中的`Thread`将无法创建其他`Process`或`Job`。


<!-- 
[Program loading](program_loading.md) is provided by userspace facilities and
protocols above the kernel layer. 
-->
[程序的加载（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/program_loading.md)机制由用户空间设施和内核层之上的协议所提供。

<!---
See: [process_create](syscalls/process_create.md),
[process_start](syscalls/process_start.md),
[thread_create](syscalls/thread_create.md),
and [thread_start](syscalls/thread_start.md). 
--->

请查看：[process_create（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/process_create.md)，[process_start（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/process_start.md)，[thread_create（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/thread_create.md)和[thread_start（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/thread_start.md)。 

<!-- ## Message Passing: Sockets and Channels -->
## 消息传递：Socket和Channel

<!---
Both Sockets and Channels are IPC Objects which are bi-directional and two-ended.
Creating a Socket or a Channel will return two Handles, one referring to each endpoint
of the Object.
--->
`Socket`和`Channel`都是双向和双端的进程间通信（IPC）相关的`Object`。创建`Socket`或`Channel`将返回两个不同的`Handle`，分别指向`Socket`或`Channel`的两端。

<!---
Sockets are stream-oriented and data may be written into or read out of them in units
of one or more bytes.  Short writes (if the Socket's buffers are full) and short reads
(if more data is requested than in the buffers) are possible.
--->
`Socket`是面向流的对象，可以通过它读取或写入以一个或多个字节为单位的数据。Short writes（如果Socket的缓冲区已满）和short read（如果请求的字节数量超过缓冲区大小）也同样受到支持。

<!---
Channels are datagram-oriented and have a maximum message size of 64K (subject to change,
likely to be smaller) and may also have up to 1024 Handles attached to a message (also
subject to change, also likely to be smaller).  They do not support short reads or writes --
either a message fits or it does not.
--->
`Channel`是面向数据包的对象，并限制消息的大小最多为64K（如果有改变，可能会更小），以及最多1024个`Handle`挂载到同一消息上（如果有改变，同样可能会更小）。无论消息是否满足short write或read的条件，`Channel`均不支持它们。

<!---
When Handles are written into a Channel, they are removed from the sending Process.
When a message with Handles is read from a Channel, the Handles are added to the receiving
Process.  Between these two events, the Handles continue to exist (ensuring the Objects
they refer to continue to exist), unless the end of the Channel which they have been written
towards is closed -- at which point messages in flight to that endpoint are discarded and
any Handles they contained are closed.
--->
当`Handle`被写入到`Channel`中时，在发送端`Process`中将会移除这些`Handle`。同时携带`Handle`的消息从`Channel`中被读取时，该`Handle`也将被加入到接收端`Process`中。在这两个时间点之间时，`Handle`将同时存在于两端（以保证它们指向的`Object`继续存在而不被销毁），除非`Channel`写入方向一端被关闭，这种情况下，指向该端点的正在发送的消息将被丢弃，并且它们包含的任何句柄都将被关闭。

<!---
See: [channel_create](syscalls/channel_create.md),
[channel_read](syscalls/channel_read.md),
[channel_write](syscalls/channel_write.md),
[channel_call](syscalls/channel_call.md),
[socket_create](syscalls/socket_create.md),
[socket_read](syscalls/socket_read.md),
and [socket_write](syscalls/socket_write.md). 
--->

请查看：[channel_create](syscalls/channel_create.md)，[channel_read](syscalls/channel_read.md)，[channel_write](syscalls/channel_write.md)，[channel_call](syscalls/channel_call.md)，[socket_create（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/socket_create.md)，[socket_read（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/socket_read.md)和[socket_write（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/socket_write.md)。

<!---
## Objects and Signals
--->
## 对象（Object）和信号（Signal）

<!---
Objects may have up to 32 signals (represented by the zx_signals_t type and the ZX_*_SIGNAL_*
defines) which represent a piece of information about their current state.  Channels and Sockets,
for example, may be READABLE or WRITABLE.  Processes or Threads may be TERMINATED.  And so on.
--->
`Object`至多可以可包含32个信号（通过`zx_signals_t`数据结构和ZX_*SIGNAL*的定义），它们是表示当前一些状态信息。例如，`Channel`和`Socket`可为可读（`READABLE`）和可写的（`WRITABLE`），`Process`或`Thread`可以为终止（`TERMINATED`）状态等等。

<!---
Threads may wait for signals to become active on one or more Objects.

See [signals](signals.md) for more information.
--->
线程可以等待一个或多个`Object`被信号激活。

请查看[signals（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/signals.md)以了解更多的信息。

<!---
## Waiting: Wait One, Wait Many, and Ports
--->
## 等待：等待一次，等待多次和端口（Port）

<!---
A Thread may use [*zx_object_wait_one()*](syscalls/object_wait_one.md)
to wait for a signal to be active on a single handle or
[*zx_object_wait_many()*](syscalls/object_wait_many.md) to wait for
signals on multiple handles.  Both calls allow for a timeout after
which they'll return even if no signals are pending.
--->
`Thread`可以使用[*zx_object_wait_one()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_wait_one.md)来等待某个句柄被收到信号，或者使用[*zx_object_wait_many()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_wait_many.md)方法来等待多个句柄收到信号。两种调用都允许设置超时点，即没有信号激活，它们也会因超时而返回。

<!---
If a Thread is going to wait on a large set of handles, it is more efficient to use
a Port, which is an Object that other Objects may be bound to such that when signals
are asserted on them, the Port receives a packet containing information about the
pending Signals.
--->
如果`Thread`需要等待大量的句柄时，一种更高效的方式是使用`Port`。`Port`是一种其他对象可以加以绑定的`Object`，绑定的结果是当`Single`被发送到`Port`上时，`Port`将会接收到包含这些`Signal`信息的数据包给`Thread`作处理。

<!---
See: [port_create](syscalls/port_create.md),
[port_queue](syscalls/port_queue.md),
[port_wait](syscalls/port_wait.md),
[port_cancel](syscalls/port_cancel.md).
--->
请查看：[port_create（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_create.md)，[port_queue（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_queue.md)，[port_wait（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_wait.md)和[port_cancel（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_cancel.md)。


<!---
## Events, Event Pairs.
--->
## 事件（Event）和事件对（Event Pair）

<!---
An Event is the simplest Object, having no other state than its collection of active Signals.
--->
`Event`是一种仅包含活跃`Singal`的集合的最简单`Object`。

<!---
An Event Pair is one of a pair of Events that may signal each other.  A useful property of
Event Pairs is that when one side of a pair goes away (all Handles to it have been
closed), the PEER_CLOSED signal is asserted on the other side.
--->
`Event Pair`是一对可相互信号通知的`Event`组合。`Event Pair`的一个有用性质是当其中一个被销毁（指向它的所有`Handle`均已关闭）时，另一个事件将接收`PEER_CLOSED`信号。
<!---
See: [event_create](syscalls/event_create.md),
and [eventpair_create](syscalls/eventpair_create.md).
--->
请查看：[event_create（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/event_create.md)和[eventpair_create（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/eventpair_create.md)。


<!---
## Shared Memory: Virtual Memory Objects (VMOs)
--->
## 共享内存： 虚拟内存对象（VMO）

<!---
Virtual Memory Objects represent a set of physical pages of memory, or the *potential*
for pages (which will be created/filled lazily, on-demand).
--->
虚拟内存对象表示一组物理内存页，或者是*潜在*的内存页（例如将被按需惰性创建/填充）。

<!---
They may be mapped into the address space of a Process with
[*zx_vmar_map()*](syscalls/vmar_map.md) and unmapped with
[*zx_vmar_unmap()*](syscalls/vmar_unmap.md).  Permissions of
mapped pages may be adjusted with [*zx_vmar_protect()*](syscalls/vmar_protect.md).
--->
VMO可以通过[*zx_vmar_map()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_map.md)被映射到某个`Process`的地址空间，或者通过[*zx_vmar_unmap()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_unmap.md)解除映射。这些映射页的权限可通过[*zx_vmar_protect()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_protect.md)函数进行调整。

<!---
VMOs may also be read from and written to directly with
[*zx_vmo_read()*](syscalls/vmo_read.md) and [*zx_vmo_write()*](syscalls/vmo_write.md).
Thus the cost of mapping them into an address space may be avoided for one-shot operations
like "create a VMO, write a dataset into it, and hand it to another Process to use."
--->
VMO可通过[*zx_vmo_read()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_read.md)和[*zx_vmo_write()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_write.md)操作对它们进行直接的读写操作。因此，通过一次合并操作可以避免映射到地址空间的开销，例如”创建VMO，向其写入数据集后，传入另一进程供其使用“。


<!---
## Address Space Management
--->
## 地址空间管理

<!---
Virtual Memory Address Regions (VMARs) provide an abstraction for managing a
process's address space.  At process creation time, a handle to the root VMAR
is given to the process creator.  That handle refers to a VMAR that spans the
entire address space.  This space can be carved up via the
[*zx_vmar_map()*](syscalls/vmar_map.md) and
[*zx_vmar_allocate()*](syscalls/vmar_allocate.md) interfaces.
[*zx_vmar_allocate()*](syscalls/vmar_allocate.md) can be used to generate new
VMARs (called subregions or children) which can be used to group together
parts of the address space.
--->
虚拟内存地址区域（Virtual Memory Address Regions，即VMAR）提供了管理进程地址空间的抽象。在进程创建时，根VMAR的句柄被提供给进程的创建者。该句柄指向一个涵盖整个地址空间的VMAR，该空间可通过[*zx_vmar_map()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_map.md)和[*zx_vmar_allocate()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_allocate.md)接口进行分割操作。同时，[*zx_vmar_allocate()*（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_allocate.md)可以被用作生成新的VMAR（被称为子区域或子代）以将地址空间的多个部分组合在一起。

<!---
See: [vmar_map](syscalls/vmar_map.md),
[vmar_allocate](syscalls/vmar_allocate.md),
[vmar_protect](syscalls/vmar_protect.md),
[vmar_unmap](syscalls/vmar_unmap.md),
[vmar_destroy](syscalls/vmar_destroy.md),
--->
请查看：[vmar_map（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_map.md)，[vmar_allocate（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_allocate.md)，[vmar_protect（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_protect.md)，[vmar_unmap（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_unmap.md)和[vmar_destroy（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_destroy.md)。

<!---
## Futexes
--->
## 快速用户区互斥锁（Futex）

<!---
Futexes are kernel primitives used with userspace atomic operations to implement
efficient synchronization primitives -- for example, Mutexes which only need to make
a syscall in the contended case.  Usually they are only of interest to implementers of
standard libraries.  Zircon's libc and libc++ provide C11, C++, and pthread APIs for
mutexes, condition variables, etc, implemented in terms of Futexes.
--->
`Futex`是用于在用户空间实现高效（例如，内核中的`Mutex`只需要在竞争处一次调用）同步原子操作的内核原语。通常情况下，只有实现标准库的开发者对它们感兴趣。在Zircon的libc和libc++中，提供了基于Futex的mutex和condition变量的实现，包括C11，C++和pthread API等版本。

<!---
See: [futex_wait](syscalls/futex_wait.md),
[futex_wake](syscalls/futex_wake.md),
[futex_requeue](syscalls/futex_requeue.md).
--->
请查看： [futex_wait（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/futex_wait.md)，[futex_wake（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/futex_wake.md)和[futex_requeue（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/futex_requeue.md)。

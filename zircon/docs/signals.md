# Zircon Signals
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/599a37ad75ad6a3e7963a808e9f05c8fd6865749/docs/signals.md)

---
<!-- ## Introduction -->
## 介绍

<!-- A signal is a single bit of information that waitable zircon kernel objects expose to
applications.  Each object can expose one or more signals; some are generic and some
are specific to the type of object. -->
信号是Zircon中可等待的内核对象开放给应用程序的位信息。 
每个对象可以提供一个或多个信号，它们中一些是通用的，另外一些是特定于对象类型的。

<!-- For example, the signal *ZX_CHANNEL_READABLE* indicates "this channel endpoint has
messages to read", and **ZX_PROCESS_TERMINATED** indicates "this process stopped running." -->
例如，信号*ZX_CHANNEL_READABLE*表示“此channel端点有要读取的消息”，**ZX_PROCESS_TERMINATED**表示“此进程已停止运行”。

<!-- The signals for an object are stored in a uint32 bitmask, and their values (which are
object-specific) are defined in the header[`zircon/types.h`](../system/public/zircon/types.h).
The typedef `zx_signals_t` is used to refer to signal bitmasks in syscalls and other APIs. -->
对象的信号存储在`uint32`类型的位掩码中，它们的值（特定于对象）在头文件[`zircon/types.h`](https://github.com/fuchsia-mirror/zircon/blob/master/system/public/zircon/types.h)中定义。
`zx_signals_t`类型用于引用系统调用和其他API中使用的信号位掩码。

<!-- Most objects are waitable.  Ports are an example of a non-waitable object.
To determine if an object is waitable, call [object_get_info](syscalls/object_get_info.md)
with **ZX_INFO_HANDLE_BASIC** topic and test for **ZX_OBJ_PROP_WAITABLE**. -->
大多数对象都是可等待的，但端口却是不可等待对象的一个例子。 
要确定对象是否是可等待的，请使用**ZX_INFO_HANDLE_BASIC**主题调用[object_get_info](syscalls/object_get_info.md)并核对**ZX_OBJ_PROP_WAITABLE**权限是否存在。

<!-- ## State, State Changes and their Terminology -->
## 状态，状态变化及其术语

<!-- A signal is said to be **Active** when its bit is 1 and **Inactive** when its bit is 0. -->
对于一个信号而言，当它的位值为1时，被称为**活跃(Active)** 状态；而在它的位值为0时，被称为**非活跃(Inactive)** 状态。

<!-- A signal is said to be **Asserted** when it is made **Active** in response to an event
(even if it was already **Active**), and is said to be **Deasserted** when it is made
**Inactive** in response to an event (even if it was already **Inactive**). -->
一个信号当它因响应一个事件而变成**活跃**状态（即使它已经处于**活跃**状态）时，被称为**置位**。
当它因响应一个事件而变成**非活跃**状态（即使它已经处于**非活跃**状态）时，被称为**取消置位**。

<!-- For example:  When a message is written into a Channel endpoint, the *ZX_CHANNEL_READABLE*
signal of the opposing endpoint is **asserted** (which causes that signal to become **active**,
if it were not already active).  When the last message in a Channel endpoint's
queue is read from that endpoint, the *ZX_CHANNEL_READABLE* signal of that endpoint is
**deasserted** (which causes that signal to become **inactive**) -->
例如：当消息被写入channel的一个端点时，反向对等端点的*ZX_CHANNEL_READABLE*信号将被**置位**（使得该信号变为**活跃**状态）。
相反地，当从该端点读取channel队列中的最后一条消息时，该端点的*ZX_CHANNEL_READABLE*信号位将被*取消置位*（这会使该信号变为**非活跃**状态）。

<!-- ## Observing Signals -->
## 获取信号

<!-- The syscalls **zx_object_wait_one**(), **zx_object_wait_many**(), and **zx_object_wait_async**() (in
combination with a Port), can be used to wait for specified signals on one or more objects. -->
系统调用**zx_object_wait_one()**，**zx_object_wait_many()** 和**zx_object_wait_async()**（与端口相结合使用时，）可用于等待一个或多个对象上指定的信号。

<!-- ## Common Signals -->
## 常见信号

### ZX_SIGNAL_HANDLE_CLOSED

<!-- 
This synthetic signal only exists in the results of [object_wait_one](syscalls/object_wait_one.md)
or [object_wait_many](syscalls/object_wait_many.md) and indicates that a handle that was
being waited upon has been been closed causing the wait operation to be aborted. -->
此合成信号仅存在于[object_wait_one](syscalls/object_wait_one.md)或[object_wait_many](syscalls/object_wait_many.md)的返回结果中，表示正在等待的句柄已被关闭，使得等待操作被中止。

<!-- 
This signal can only be obtained as a result of the above two wait calls when the wait itself
returns with **ZX_ERR_CANCELED**. -->
当等待本身返回**ZX_ERR_CANCELED**时，只能通过上述两种等待调用获得此信号。

<!-- ## User Signals -->
## 用户信号

<!-- There are eight User Signals (**ZX_USER_SIGNAL_0** through **ZX_USER_SIGNAL_7**) which may
asserted or deasserted using the **zx_object_signal**() and **zx_object_signal_peer**() syscalls,
provided the handle has the appropriate rights (**ZX_RIGHT_SIGNAL** or **ZX_RIGHT_SIGNAL_PEER**,
respectively).  These User Signals are always initially inactive, and are only modified by
the object signal syscalls. -->
系统中有八个用户信号（从**ZX_USER_SIGNAL_0**到**ZX_USER_SIGNAL_7**），可以使用**zx_object_signal()**和**zx_object_signal_peer()** 系统调用来使这些信号置位或解除置位，前提是句柄具有相应的权限（分别为**ZX_RIGHT_SIGNAL**或**ZX_RIGHT_SIGNAL_PEER**）。 
这些用户信号在初始状态始终是非活跃状态，并且仅能通过对象信号的系统调用进行修改。

<!-- ## See Also -->
## 另见

[object_signal](syscalls/object_signal.md),
[object_signal_peer](syscalls/object_signal.md),
[object_wait_async](syscalls/object_wait_async.md),
[object_wait_many](syscalls/object_wait_many.md),
[object_wait_one](syscalls/object_wait_one.md).
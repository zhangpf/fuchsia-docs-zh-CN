# zx_object_signal, zx_object_signal_peer
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_signal.md)

---
<!-- ## NAME -->
## 名称

<!-- object_signal - signal an object -->
object_signal —— 给对象发信号

<!-- object_signal_peer - signal an object's peer -->
object_signal_peer —— 给对象的对等端点发信号

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_object_signal(zx_handle_t handle, uint32_t clear_mask, uint32_t set_mask);
zx_status_t zx_object_signal_peer(zx_handle_t handle, uint32_t clear_mask, uint32_t set_mask);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_object_signal**() and **zx_object_signal_peer**() assert and deassert the
userspace-accessible signal bits on an object or on the object's peer,
respectively. A object peer is the opposite endpoint of a *channel*, *socket*,
*fifo*, or *eventpair*. -->
**zx_object_signal()** 和**zx_object_signal_peer()** 分别用于在对象或对象的对等端点上的用户空间可访问的信号上进行置位和取消置位操作。 
对象的对等端点包括*通道(channel)*，*socket*，*fifo*或*事件对(eventpair)* 的反方向相对端点。

<!-- Most of the 32 signals are reserved for system use and are assigned to
per-object functions, like *ZX_CHANNEL_READABLE* or *ZX_TASK_TERMINATED*. There
are 8 signal bits available for userspace processes to use as they see fit:
*ZX_USER_SIGNAL_0* through *ZX_USER_SIGNAL_7*. -->
32个信号中的大多数保留供系统使用，并分配给每个对象相应的功能，如*ZX_CHANNEL_READABLE*或*ZX_TASK_TERMINATED*等。 
如果用户程序需要的话，其中有8个信号位可供用户空间的进程使用：*ZX_USER_SIGNAL_0*到*ZX_USER_SIGNAL_7*。

<!-- *Event* objects also allow control over the *ZX_EVENT_SIGNALED* bit. -->
<!-- *Eventpair* objects also allow control over the *ZX_EVENTPAIR_SIGNALED* bit. -->
此外，*事件(event)* 对象允许控制*ZX_EVENT_SIGNALED*位，以及*事件对(eventpair)* 对象允许控制*ZX_EVENTPAIR_SIGNALED*位。

<!-- The *clear_mask* is first used to clear any bits indicated, and then the
*set_mask* is used to set any bits indicated. -->
首先，使用*clear_mask*清除所有指定的位，然后使用*set_mask*设置所有指定的位。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_object_signal**() and **zx_object_signal_peer**() return **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。
<!-- 
**ZX_ERR_ACCESS_DENIED**  *handle* lacks the right **ZX_RIGHT_SIGNAL** (for **zx_object_signal**()) or
**ZX_RIGHT_SIGNAL_PEER** (for **zx_object_signal_peer**()). -->
**ZX_ERR_ACCESS_DENIED**：（对于**zx_object_signal()** 来讲）*handle*缺少**ZX_RIGHT_SIGNAL**权限，或（对于**zx_object_signal_peer()** 来讲）缺少**ZX_RIGHT_SIGNAL_PEER**权限。

<!-- **ZX_ERR_INVALID_ARGS**  *clear_mask* or *set_mask* contain bits that are not allowed. -->
**ZX_ERR_INVALID_ARGS**：*clear_mask*或*set_mask*包含无效位。

<!-- **ZX_ERR_NOT_SUPPORTED**  **zx_object_signal_peer**() used on an object lacking a peer. -->
**ZX_ERR_NOT_SUPPORTED**：在缺少对等端点的对象上调用**zx_object_signal_peer()**。

<!-- **ZX_ERR_PEER_CLOSED**  **zx_object_signal_peer**() called on an object with a closed peer. -->
**ZX_ERR_PEER_CLOSED**：在对等端点已关闭的对象上调用**zx_object_signal_peer()**。

<!-- ## NOTE -->
## 注释

<!-- *ZX_RIGHT_WRITE* is used to gate access to signal bits.  This will likely change. -->
*ZX_RIGHT_WRITE*用于控制对位信号的访问，但这在未来可能会发生改变。

<!-- ## SEE ALSO -->
## 另见

[event_create](event_create.md),
[eventpair_create](eventpair_create.md).
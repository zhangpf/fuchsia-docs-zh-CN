# zx_object_wait_one
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_wait_one.md)

---
<!-- ## NAME -->
## 名称

<!-- object_wait_one - wait for signals on an object -->
object_wait_many —— 等待单个对象发出的信号

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_object_wait_one(zx_handle_t handle,
                               zx_signals_t signals,
                               zx_time_t deadline,
                               zx_signals_t* observed);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **object_wait_one**() is a blocking syscall which causes the caller to
wait until either the *deadline* passes or the object to which *handle* refers
asserts at least one of the specified *signals*. If the object is already
asserting at least one of the specified *signals*, the wait ends immediately. -->
**object_wait_one()** 是一个阻塞式的系统调用，它的目的是使调用者等待直到截止时刻*deadline*的到来，或者*signals*中至少有一个指定的信号被句柄所引用的对象所置位。
如果对象已经在*signals*中至少一个指定信号上置位，则等待立即返回。

<!-- Upon return, if non-NULL, *observed* is a bitmap of *all* of the
signals which were observed asserted on that object while waiting. -->
在返回时，如果*observed*为非`NULL`，则它是*所有*等待的该对象上所有已置位信号的位图(bitmap)。

<!-- The *observed* signals may not reflect the actual state of the object's
signals if the state of the object was modified by another thread or
process.  (For example, a Channel ceases asserting **ZX_CHANNEL_READABLE**
once the last message in its queue is read). -->
如果对象的状态被另一个线程或进程修改，则*observed*中的*pending*信号可能无法反映对象信号的实际状态。 
（例如，一旦队列中的最后一条消息被读取，通道就会停止在**ZX_CHANNEL_READABLE**上置位）

<!-- The *deadline* parameter specifies a deadline with respect to
**ZX_CLOCK_MONOTONIC**.  **ZX_TIME_INFINITE** is a special value meaning wait
forever. -->
*deadline*参数指定了相对于**ZX_CLOCK_MONOTONIC**的截止时刻。 
而**ZX_TIME_INFINITE**参数是一个特殊的值，它表示永远等待。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **object_wait_one**() returns **ZX_OK** if any of *signals* were observed
on the object before *deadline* passes. -->
如果在*deadline*截至时刻到来之前，在对象上观察到任何*signals*信号中的一个，则**object_wait_one()** 返回**ZX_OK**。


<!-- In the event of **ZX_ERR_TIMED_OUT**, *observed* may reflect state changes
that occurred after the deadline passed, but before the syscall returned. -->
即使调用返回**ZX_ERR_TIMED_OUT**，*observed*也会反映出在截止时刻到来之后但在系统调用返回之前发生的状态更改。

<!-- For any other return value, *observed* is undefined. -->
对于任何其他返回值，*observed*均是未定义的。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *observed* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*observe*是无效指针。

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_WAIT** and may
not be waited upon. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_WAIT**权限，因此无法被用于等待。

<!-- **ZX_ERR_CANCELED**  *handle* was invalidated (e.g., closed) during the wait. -->
**ZX_ERR_CANCELED**：*handle*在等待期间失效（例如关闭）。

<!-- **ZX_ERR_TIMED_OUT**  The specified deadline passed before any of the specified
*signals* are observed on *handle*. -->
**ZX_ERR_TIMED_OUT**：指定的截止时刻超时发生在，句柄*handle*上等待到*signals*中任何信号之前。

<!-- **ZX_ERR_NOT_SUPPORTED**  *handle* is a handle that cannot be waited on
(for example, a Port handle). -->
**ZX_ERR_NOT_SUPPORTED**：*handle*其中一项包含不可等待的句柄（例如端口句柄等）。

<!-- ## SEE ALSO -->
## 另见

[object_wait_async](object_wait_async.md),
[object_wait_many](object_wait_many.md).
# zx_object_wait_many
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_wait_many.md)

---
<!-- ## NAME -->
## 名称

<!-- object_wait_many - wait for signals on multiple objects -->
object_wait_many —— 等待多个对象上发出的信号

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_object_wait_many(zx_wait_item_t* items, size_t count, zx_time_t deadline);

typedef struct {
    zx_handle_t handle;
    zx_signals_t waitfor;
    zx_signals_t pending;
} zx_wait_item_t;
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **object_wait_many**() is a blocking syscall which causes the caller to
wait until either the *deadline* passes or at least one of the specified
signals is asserted by the object to which the associated handle refers.
If an object is already asserting at least one of the specified signals,
the wait ends immediately. -->
**object_wait_many()** 是一个阻塞式的系统调用，它的目的是使调用者等待直到截止时刻*deadline*到来，或者指定的信号中至少有一个被相关句柄所引用的对象置位。
如果对象已经在至少一个指定信号上置位，则等待立即返回。

<!-- The caller must provide *count* zx_wait_item_ts in the *items* array,
containing the handle and signals bitmask to wait for for each item. -->
调用者必须在*items*数组中提供*count*个`zx_wait_item_ts`类型的结构体，其中每一项包含句柄和信号位掩码以用于等待操作。

<!-- The *deadline* parameter specifies a deadline with respect to
**ZX_CLOCK_MONOTONIC**.  **ZX_TIME_INFINITE** is a special value meaning wait
forever. -->
*deadline*参数指定了相对于**ZX_CLOCK_MONOTONIC**的截止时刻。 
而**ZX_TIME_INFINITE**参数是一个特殊的值，它表示永远等待。

<!-- Upon return, the *pending* field of *items* is filled with bitmaps indicating
which signals are pending for each item. -->
调用返回时，*items*的*pending*字段用表示每个项的待处理信号的位图(bitmap)来填充。

<!-- The *pending* signals in *items* may not reflect the actual state of the object's
signals if the state of the object was modified by another thread or
process.  (For example, a Channel ceases asserting **ZX_CHANNEL_READABLE**
once the last message in its queue is read). -->
如果对象的状态被另一个线程或进程修改，则*items*中的*pending*信号可能无法反映对象信号的实际状态。 
（例如，一旦队列中的最后一条消息被读取，通道就会停止在**ZX_CHANNEL_READABLE**上置位）

<!-- The maximum number of items that may be waited upon is **ZX_WAIT_MANY_MAX_ITEMS**,
which is 8.  To wait on more things at once use [Ports](../objects/port.md). -->
可以用于等待的最大项数为**ZX_WAIT_MANY_MAX_ITEMS**，即为8。
如果需要等待更多对象，请使用[端口](../objects/port.md)。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **object_wait_many**() returns **ZX_OK** if any of *waitfor* signals were
observed on their respective object before *deadline* passed. -->
如果在*deadline*截止时刻到来之前，在其各自的对象上观察到任何*waitfor*信号中的一个，则**object_wait_many()** 返回**ZX_OK**。

<!-- In the event of **ZX_ERR_TIMED_OUT**, *items* may reflect state changes
that occurred after the deadline pased, but before the syscall returned. -->
即使调用返回**ZX_ERR_TIMED_OUT**，*items*也会反映出在截止时刻到来之后但在系统调用返回之前发生的状态更改。

<!-- In the event of **ZX_ERR_CANCELED**, one or more of the items being waited
upon have had their handles closed, and the *pending* field for those items
will have the **ZX_SIGNAL_HANDLE_CLOSED** bit set. -->
如果返回**ZX_ERR_CANCELED**，表示一个或多个等待的句柄已关闭，并且这些项的*pending*字段将置**ZX_SIGNAL_HANDLE_CLOSED**位。

<!-- For any other return value, the *pending* fields of *items* are undefined. -->
对于任何其他返回值，*items*的*pending*字段均是未定义的。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *items* isn't a valid pointer. -->
**ZX_ERR_INVALID_ARGS**：*items*是无效指针。

<!-- **ZX_ERR_OUT_OF_RANGE**  *count* is greater than **ZX_WAIT_MANY_MAX_ITEMS**. -->
**ZX_ERR_OUT_OF_RANGE**：*count*大于**ZX_WAIT_MANY_MAX_ITEMS**。
<!-- 
**ZX_ERR_BAD_HANDLE**  one of *items* contains an invalid handle. -->
**ZX_ERR_BAD_HANDLE**：*items*其中一项包含无效句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  One or more of the provided *handles* does not
have **ZX_RIGHT_WAIT** and may not be waited upon. -->
**ZX_ERR_ACCESS_DENIED**：提供的*handles*中一个或多个没有**ZX_RIGHT_WAIT**权限，因此可能无法等待。

<!-- 
**ZX_ERR_CANCELED**  One or more of the provided *handles* was invalidated
(e.g., closed) during the wait. -->
**ZX_ERR_CANCELED**：在等待期间，提供的*handles*中一个或多个变成无效状态（比如关闭）。

<!-- **ZX_ERR_TIMED_OUT**  The specified deadline passed before any of the specified signals are
observed on any of the specified handles. -->
**ZX_ERR_TIMED_OUT**：指定的截止时刻超时发生在，任何指定句柄上等待到任何指定信号之前。

<!-- 
**ZX_ERR_NOT_SUPPORTED**  One of the *items* contains a handle that cannot
be waited one (for example, a Port handle). -->
<!-- TODO one ===> on -->
**ZX_ERR_NOT_SUPPORTED**：*items*其中一项包含不可等待的句柄（例如端口句柄等）。

<!-- 
**ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

## BUGS

<!-- *pending* more properly should be called *observed*. -->
参数名称*pending*应该更合适被称作*observed*。

<!-- ## SEE ALSO -->
## 另见

[object_wait_many](object_wait_many.md),
[object_wait_one](object_wait_one.md).
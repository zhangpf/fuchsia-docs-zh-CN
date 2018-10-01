# zx_object_wait_async
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_wait_async.md)

---
<!-- ## NAME -->
## 名称

<!-- object_wait_async - subscribe for signals on an object -->
object_wait_async —— 订阅对象发出信号

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_object_wait_async(zx_handle_t handle,
                                 zx_handle_t port,
                                 uint64_t key,
                                 zx_signals_t signals,
                                 uint32_t options);
```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**object_wait_async**() is a non-blocking syscall which causes packets to be
enqueued on *port* when the the specified condition is met.
Use **port_wait**() to retrieve the packets. -->
**object_wait_async()** 是一个非阻塞的系统调用，当满足指定的条件时，系统会从*port*端口将数据包排入队列。 
而后可使用**port_wait()** 来获取数据包。

<!-- *handle* points to the object that is to be watched for changes and must be a waitable object. -->
*handle*指向要订阅其修改状态的对象，并且该对象必须是可等待的。

<!-- The *options* argument can be either **ZX_WAIT_ASYNC_ONCE** or **ZX_WAIT_ASYNC_REPEATING**. -->
*options*参数可以是**ZX_WAIT_ASYNC_ONCE**或**ZX_WAIT_ASYNC_REPEATING**。

<!-- In both cases, *signals* indicates which signals on the object specified by *handle*
will cause a packet to be enqueued, and if **any** of those signals are asserted when
**object_wait_async**() is called, or become asserted afterwards, a packet will be
enqueued on *port*. -->
在这两种情况下，*signals*表示由*handle*指定的对象上的哪些信号会将数据包排入队列，并且如果在调用**object_wait_async()** 时将置位这些信号中**任何**之一，或在之后被置位，数据包都将从*port*端口上被排入队列中。

<!-- In the case of **ZX_WAIT_ASYNC_ONCE**, once a packet has been enqueued the asynchronous
waiting ends.  No further packets will be enqueued. -->
在包含**ZX_WAIT_ASYNC_ONCE**选项的情况下，一旦将数据包排入队列，异步等待立即结束，并且不会有其他数据包再排入队列。

<!-- 
In the case of **ZX_WAIT_ASYNC_REPEATING** the asynchronous waiting continues until
canceled. If any of *signals* are asserted and a packet is not currently in *port*'s
queue on behalf of this wait, a packet is enqueued. If a packet is already in the
queue, the packet's *observed* field is updated. -->
在包含**ZX_WAIT_ASYNC_REPEATING**选项的情况下，将继续异步等待直至取消操作。
如果*signals*中的任何之一被置位，且数据包当前不在表示此等待的*port*队列中，则该数据包也将被排入队列。
如果数据包已经在队列中，则数据包的*observed*字段将被更新。

<!-- In either mode, **port_cancel**() will terminate the operation and if a packet was
in the queue on behalf of the operation, that packet will be removed from the queue. -->
在上述任一模式下，**port_cancel()** 调用将取消操作。
如果该操作发生时数据包已在队列中，则该数据包将从队列中被删除。

<!-- If the handle is closed, the operation will also be terminated, but packets already
in the queue are not affected. -->
如果关闭句柄，该订阅操作也将终止，但队列中已有的数据包不受影响。

<!-- See [port_wait](port_wait.md) for more information about each type
of packet and their semantics. -->
有关每种类型的数据包及其语义的更多信息，请参见[port_wait](port_wait.md)。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值
<!-- 
**object_wait_async**() returns **ZX_OK** if the subscription succeeded. -->
如果订阅成功，则**object_wait_async()** 返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *options* is not **ZX_WAIT_ASYNC_ONCE** or **ZX_WAIT_ASYNC_REPEATING**. -->
**ZX_ERR_INVALID_ARGS**：*options*不是**ZX_WAIT_ASYNC_ONCE**或**ZX_WAIT_ASYNC_REPEATING**。

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle or *port* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄或*port*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *port* is not a Port handle. -->
**ZX_ERR_WRONG_TYPE**：*port*不是端口类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_WAIT** or *port*
does not have **ZX_RIGHT_WRITE**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_WAIT**权限，或*port*没有**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_NOT_SUPPORTED**  *handle* is a handle that cannot be waited on. -->
**ZX_ERR_NOT_SUPPORTED**：*handle*是一个不可等待的句柄。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- ## NOTES -->
## 注释

<!-- See [signals](../signals.md) for more information about signals and their terminology. -->
有关信号及其术语的更多信息，请参见[signals（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/signals.md)。

<!-- ## SEE ALSO -->
## 另见

[port_cancel](port_cancel.md).
[port_queue](port_queue.md).
[port_wait](port_wait.md).

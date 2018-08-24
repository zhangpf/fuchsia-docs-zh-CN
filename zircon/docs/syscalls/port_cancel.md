# zx_port_cancel
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/d17af78df889107ed5035b3f420567675a3c6ee5/docs/syscalls/port_cancel.md)

---
<!-- ## NAME -->
## 名称

<!-- port_cancel - cancels async port notifications on an object -->
port_cancel —— 取消对象上的异步端口通知

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_port_cancel(zx_handle_t port,
                           zx_handle_t source,
                           uint64_t key);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **port_cancel**() is a non-blocking syscall which cancels
pending **object_wait_async**() calls done with *source* and *key*. -->
**port_cancel()** 是一个非阻塞的系统调用，它用*source*和*key*取消待处理的**object_wait_async()** 调用。

<!-- 
When this call succeeds no new packets from the object pointed by
*source* with *key* will be delivered to *port*, and pending queued
packets that match *source* and *key* are removed from the port. -->
当此调用成功时，由*source*对象发出且键值为*key*的新数据包将不再传递到*port*，并且与*source*和*key*匹配的待处理数据包也从端口中被删除。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_port_cancel**() returns **ZX_OK** if cancellation succeeded and
either queued packets were removed or pending **object_wait_async**() were
canceled. -->

**zx_port_cancel()** 如果取消成功，并删除排队中的数据包，或待处理的**object_wait_async()** 调用被取消，则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *source* or *port* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*source*或*port*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *port* is not a port handle. -->
**ZX_ERR_WRONG_TYPE**：*port*不是端口类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *source* or *port* does not have **ZX_RIGHT_WRITE**. -->
**ZX_ERR_ACCESS_DENIED**：*source*或*port*没有**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_NOT_SUPPORTED**  *source* is a handle that cannot be waited on. -->
**ZX_ERR_NOT_SUPPORTED**：*source*是一个不可等待的句柄。

<!-- **ZX_ERR_NOT_FOUND** if either no pending packets or pending
**object_wait_async** calls with *source* and *key* were found. -->
**ZX_ERR_NOT_FOUND**：找不到待处理的数据包和待处理的以*source*和*key*为参数的**object_wait_async**调用。

<!-- ## SEE ALSO -->
## 另见

<!-- [port_wait](port_wait.md). -->

[port_wait](port_wait.md)。
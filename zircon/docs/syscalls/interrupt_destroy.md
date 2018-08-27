# zx_interrupt_destroy
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fd7c237ca14f48dd4f1d7a0a7c3d2d3d304dbb47/docs/syscalls/interrupt_destroy.md)

---
<!-- ## NAME -->
## 名称

<!-- interrupt_destroy - destroys an interrupt object -->
interrupt_destroy —— 销毁中断对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_interrupt_destroy(zx_handle_t handle);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **interrupt_destroy**() "destroys" an interrupt object, putting it in a state
where any **interrupt_wait**() operations on it will return ZX_ERR_CANCELED,
and it is unbound from any ports it was bound to. -->
**interrupt_destroy()** 的功能是“销毁”一个中断对象，使其处于对其执行任何**interrupt_wait()** 操作都将返回ZX_ERR_CANCELED的状态，并且将它从绑定到的任何端口上解除绑定。

<!-- This provides a clean shut down mechanism.  Closing the last handle to the
interrupt object results in similar cancelation but could result in use-after-close
of the handle. -->
该调用提供了一种干净的关闭机制。
关闭中断对象的最后一个句柄会导致类似的取消操作，但可能会导致句柄发生使用后关闭(use-after-close)的情况。

<!-- 
If the interrupt object is bound to a port when cancelation happens, if it
has not yet triggered, or it has triggered but the packet has not yet been
received by a caller of **port_wait**(), success is returned and any packets
in flight are removed.  Otherwise, **ZX_ERR_NOT_FOUND** is returned, indicating
that the packet has been read but the interrupt has not been re-armed by calling
**zx_interrupt_ack**(). -->
如果在取消发生时中断对象已被绑定到某端口，但它尚未触发，或者它已触发但是仍然没有被**port_wait()** 调用的调用者接收到该数据包，则取消成功并且任何尚在传递过程的数据包将被删除。
否则，将返回**ZX_ERR_NOT_FOUND**，表示已读取该数据包，但调用**interrupt_ack()** 重新设置中断。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- 
**interrupt_destroy**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**interrupt_destroy()** 调用成功返回**ZX_OK**。如果调用失败，则返回负的错误码。


## ERRORS

<!-- **ZX_ERR_BAD_HANDLE** *handle* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**： *handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** *handle* is not an interrupt object. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是中断类型对象。

<!-- **ZX_ERR_NOT_FOUND**  *handle* was bound (and now no longer is) but was not
being waited for. -->
**ZX_ERR_NOT_FOUND**：*handle*被绑定（但现在已不再是）但是没有被等待。

<!-- **ZX_ERR_ACCESS_DENIED** *handle* lacks **ZX_RIGHT_WRITE**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少**ZX_RIGHT_WRITE**权限。

<!-- ## SEE ALSO -->
## 另见

[interrupt_ack](interrupt_ack.md),
[interrupt_bind](interrupt_bind.md),
[interrupt_create](interrupt_create.md),
[interrupt_trigger](interrupt_trigger.md),
[interrupt_wait](interrupt_wait.md),
[port_wait](port_wait.md),
[handle_close](handle_close.md).
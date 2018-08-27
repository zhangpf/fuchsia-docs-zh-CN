# zx_interrupt_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fd7c237ca14f48dd4f1d7a0a7c3d2d3d304dbb47/docs/syscalls/interrupt_create.md)

---
<!-- ## NAME -->
## 名称

<!-- interrupt_create - create an interrupt object -->
interrupt_create —— 创建中断对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_interrupt_create(zx_handle_t src_obj, uint32_t src_num,
                                uint32_t options, zx_handle_t* out_handle);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **interrupt_create**() creates an interrupt object which represents a physical
or virtual interrupt. -->
**interrupt_create()** 创建一个表示物理或虚拟中断的中断对象。
<!-- 
If *options* is **ZX_INTERRUPT_VIRTUAL**, *src_obj* and *src_num* are ignored and
a virtual interrupt is returned. -->

如果*options*的值是**ZX_INTERRUPT_VIRTUAL**，则忽略*src_obj*和*src_num*并返回虚拟中断。

<!-- Otherwise *src_obj* must be a suitable resource for creating platform interrupts
or a PCI object, and *src_num* is the associated interrupt number.  This restricts
the creation of interrupts to the internals of the DDK (driver development kit).
Physical interrupts are obtained by drivers through various DDK APIs. -->
否则*src_obj*必须是用于创建平台中断或PCI对象的合适资源，并且*src_num*是相关的中断号。
这限定了DDK（驱动程序开发工具包）内部的中断创建过程。
驱动程序可通过各种DDK相关的API获取物理中断。

<!-- Physical interrupts honor the options **ZX_INTERRUPT_EDGE_LOW**, **ZX_INTERRUPT_EDGE_HIGH**,
**ZX_INTERRUPT_LEVEL_LOW**, **ZX_INTERRUPT_LEVEL_HIGH**, and **ZX_INTERRUPT_REMAP_IRQ**. -->
物理中断可使用的选项包括**ZX_INTERRUPT_EDGE_LOW**，**ZX_INTERRUPT_EDGE_HIGH**，
**ZX_INTERRUPT_LEVEL_LOW**，**ZX_INTERRUPT_LEVEL_HIGH**和**ZX_INTERRUPT_REMAP_IRQ**。

<!-- The handles will have *ZX_RIGHT_INSPECT*, *ZX_RIGHT_DUPLICATE*, *ZX_RIGHT_TRANSFER*
(allowing them to be sent to another process via channel write), *ZX_RIGHT_READ*,
*ZX_RIGHT_WRITE* (required for **interrupt_ack**()), *ZX_RIGHT_WAIT* (required for
**interrupt_wait**(), and *ZX_RIGHT_SIGNAL* (required for **interrupt_trigger**()). -->
返回句柄将具有*ZX_RIGHT_INSPECT*，*ZX_RIGHT_DUPLICATE*，*ZX_RIGHT_TRANSFER*（允许它们通过通道发送到另一个进程），*ZX_RIGHT_READ*，*ZX_RIGHT_WRITE*（**interrupt_ack()** 所需），*ZX_RIGHT_WAIT*（**interrupt_wait()** 所需）和*ZX_RIGHT_SIGNAL*（**interrupt_trigger()** 所需）权限。

<!-- Interrupts are said to be "triggered" when the underlying physical interrupt occurs
or when **interrupt_trigger**() is called on a virtual interrupt.  A triggered interrupt,
when bound to a port with **interrupt_bind**(), causes a packet to be delivered to the port. -->
当底层的物理中断发生或在虚拟中断上调用**interrupt_trigger()** 时，中断被称为“触发(triggered)”。
当被触发的中断通过**interrupt_bind()** 绑定到某端口时，会导致数据包传递到该端口。

<!-- If not bound to a port, an interrupt object may be waited on with **interrupt_wait**(). -->
如果未绑定到任何端口，则可以使用**interrupt_wait()** 等待中断对象。

<!-- Interrupts cannot be waited on with the **object_wait_** family of calls. -->
中断对象无法使用**object_wait_** 系列调用来进行等待。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **interrupt_create**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**interrupt_create()** 调用成功返回**ZX_OK**。如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- 
**ZX_ERR_BAD_HANDLE** the *src_obj* handle is invalid (if this is not a virtual interrupt) -->
**ZX_ERR_BAD_HANDLE**：*src_obj*是无效句柄（，如果不是创建虚拟中断的调用）。

<!-- **ZX_ERR_WRONG_TYPE** the *src_obj* handle is not of an appropriate type to create an interrupt. -->
**ZX_ERR_WRONG_TYPE**：*src_obj*句柄不适合用于创建中断。

<!-- **ZX_ERR_ACCESS_DENIED** the *src_obj* handle does not allow this operation. -->
**ZX_ERR_ACCESS_DENIED**：*src_obj*句柄不允许此操作。

<!-- **ZX_ERR_INVALID_ARGS** *options* contains invalid flags or the *out_handle*
parameter is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*options*包含无效标志或*out_handle*参数是无效指针。

<!-- 
**ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。
<!-- ## SEE ALSO -->
## 另见

[interrupt_ack](interrupt_ack.md),
[interrupt_bind](interrupt_bind.md),
[interrupt_destroy](interrupt_destroy.md),
[interrupt_wait](interrupt_wait.md),
[port_wait](port_wait.md),
[handle_close](handle_close.md).
# zx_port_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/d17af78df889107ed5035b3f420567675a3c6ee5/docs/syscalls/port_create.md)

---
<!-- ## NAME -->
## 名称

<!-- port_create - create an IO port -->
port_create —— 创建IO端口

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_port_create(uint32_t options, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **port_create**() creates an port; a waitable object that can be used to
read packets queued by kernel or by user-mode. -->

**port_create()** 创建一个端口——一个用于读取内核或用户空间排队的数据包的可等待对象。

<!-- *options* must be **0**. -->
*options*必须为**0**。

<!-- The returned handle will have ZX_RIGHT_TRANSFER (allowing them to be sent
to another process via channel write), ZX_RIGHT_WRITE (allowing
packets to be queued), ZX_RIGHT_READ (allowing packets to be read) and
ZX_RIGHT_DUPLICATE (allowing them to be duplicated). -->

调用返回的句柄具有ZX_RIGHT_TRANSFER（允许它们通过写入channel的方式被发送到另一个进程），ZX_RIGHT_WRITE（允许将数据包排队），ZX_RIGHT_READ（允许读取数据包）和ZX_RIGHT_DUPLICATE（允许端口句柄被复制）的权限。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **port_create**() returns ZX_OK and a valid IO port handle via *out* on
success. In the event of failure, an error value is returned. -->

**port_create()** 调用成功则返回ZX_OK，以及通过*out*返回有效的IO端口句柄。如果调用失败，则返回错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS** *options* has an invalid value, or *out* is an
invalid pointer or NULL. -->
**ZX_ERR_INVALID_ARGS**：*options*具有无效值，或者*out*是无效指针或NULL。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

<!-- [port_queue](port_queue.md),
[port_wait](port_wait.md),
[object_wait_async](object_wait_async.md),
[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[handle_replace](handle_replace.md). -->

[port_queue](port_queue.md)，[port_wait](port_wait.md)，[object_wait_async](object_wait_async.md)，[handle_close](handle_close.md),[handle_duplicate](handle_duplicate.md)，[handle_replace](handle_replace.md)。
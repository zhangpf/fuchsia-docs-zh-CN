# zx_eventpair_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/eventpair_create.md)

---
<!-- ## NAME -->
## 名称

<!-- eventpair_create - create an event pair -->
eventpair_create —— 创建一对event pair。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_eventpair_create(uint32_t options, zx_handle_t* out0, zx_handle_t* out1);
```


<!-- ## DESCRIPTION -->
## 描述

<!-- **eventpair_create**() creates an event pair, which is a pair of objects that
are mutually signalable. -->
**eventpair_create()** 创建一对可相互信号通知的event pair对象。
<!-- 
The signals *ZX_EVENTPAIR_SIGNALED* and *ZX_USER_SIGNAL_n* (where *n* is 0 through 7)
may be set or cleared using **object_signal**() (modifying the signals on the
object itself), or **object_signal_peer**() (modifying the signals on its
counterpart). -->
可以使用**object_signal()**（修改对象本身上的信号）或**object_signal_peer()**（修改其相对event对象的信号），来设置或清除*ZX_EVENTPAIR_SIGNALED*和*ZX_USER_SIGNAL_n*（其中*n*是0到7）信号。

<!-- When all the handles to one of the objects have been closed, the
*ZX_EVENTPAIR_PEER_CLOSED* signal will be asserted on the opposing object. -->
当其中一个event对象的所有句柄都已关闭时，*ZX_EVENTPAIR_PEER_CLOSED*信号将在相对对象上被设置。

<!-- The newly-created handles will have the *ZX_RIGHT_TRANSFER*,
*ZX_RIGHT_DUPLICATE*, *ZX_RIGHT_READ*, *ZX_RIGHT_WRITE*, *ZX_RIGHT_SIGNAL*,
and *ZX_RIGHT_SIGNAL_PEER* rights. -->
新创建的句柄具有*ZX_RIGHT_TRANSFER*，*ZX_RIGHT_DUPLICATE*，*ZX_RIGHT_READ *，*ZX_RIGHT_WRITE*，*ZX_RIGHT_SIGNAL*和*ZX_RIGHT_SIGNAL_PEER*权限。

<!-- Currently, no options are supported, so *options* must be set to 0. -->
目前，尚不支持任何选项，因此*options*必须设置为0。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **eventpair_create**() returns **ZX_OK** on success. On failure, a (negative)
error code is returned. -->
**eventpair_create()** 调用成功则返回**ZX_OK**。失败时，返回（负的）错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *out0* or *out1* is an invalid pointer or NULL. -->
**ZX_ERR_INVALID_ARGS**：*out0*或*out1*是无效指针或NULL。

<!-- **ZX_ERR_NOT_SUPPORTED**  *options* has an unsupported flag set (i.e., is not 0). -->
**ZX_ERR_NOT_SUPPORTED**：*options*设置了不支持的标志位（即*options*不是0）。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

<!-- [event_create](event_create.md),
[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[handle_replace](handle_replace.md),
[object_signal](object_signal.md),
[object_signal_peer](object_signal.md). -->
[event_create](event_create.md)，[handle_close](handle_close.md)，[handle_duplicate](handle_duplicate.md)，[object_wait_one](object_wait_one.md)，[object_wait_many](object_wait_many.md)，[handle_replace](handle_replace.md)，[object_signal](object_signal.md)，[object_signal_peer](object_signal.md)。

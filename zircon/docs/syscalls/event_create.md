# zx_event_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/event_create.md)

---
<!-- ## NAME -->
## 名称
<!-- 
event_create - create an event -->
event_create —— 创建event对象

<!-- ## SYNOPSIS -->
## 概览

```
#include <zircon/syscalls.h>

zx_status_t zx_event_create(uint32_t options, zx_handle_t* out);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **event_create**() creates an event, which is an object that is signalable. That
is, its *ZX_USER_SIGNAL_n* (where *n* is 0 through 7) signals can be
manipulated using **object_signal**(). -->
**event_create()** 创建一个event对象，该对象是可通知的。也就是说，可以使用**object_signal()** 来操纵其*ZX_USER_SIGNAL_n*（其中*n*是0到7）信号。

<!-- The newly-created handle will have the *ZX_RIGHT_TRANSFER*, *ZX_RIGHT_DUPLICATE*,
*ZX_RIGHT_READ*, *ZX_RIGHT_WRITE*, and *ZX_RIGHT_SIGNAL* rights. -->
新创建的句柄将具有*ZX_RIGHT_TRANSFER*，*ZX_RIGHT_DUPLICATE*，*ZX_RIGHT_READ*，*ZX_RIGHT_WRITE*和*ZX_RIGHT_SIGNAL*权限。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **event_create**() returns ZX_OK and a valid event handle (via *out*) on success.
On failure, an error value is returned. -->
**event_create()** 成功则返回ZX_OK和（通过*out*）返回有效的event句柄。失败时，则返回错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *out* is an invalid pointer, or *options* is nonzero. -->
**ZX_ERR_INVALID_ARGS**： *out*是无效指针，或*options*非零。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

<!-- [eventpair_create](eventpair_create.md),
[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[handle_replace](handle_replace.md),
[object_signal](object_signal.md). -->

[eventpair_create](eventpair_create.md)，[handle_close](handle_close.md), [handle_duplicate](handle_duplicate.md)，[object_wait_one](object_wait_one.md)，[object_wait_many](object_wait_many.md)，[handle_replace](handle_replace.md)，[object_signal](object_signal.md)。

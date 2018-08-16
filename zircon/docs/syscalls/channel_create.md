# zx_channel_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/channel_create.md)

---
<!-- ## NAME -->
## 名称

<!-- channel_create - create a channel -->
channel_create —— 创建channel

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_channel_create(uint32_t options,
                              zx_handle_t* out0, zx_handle_t* out1);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **channel_create**() creates a channel, a bi-directional
datagram-style message transport capable of sending raw data bytes
as well as handles from one side to the other. -->
**channel_create()** 创建一个双向数据报式的消息传输通道，能够从一侧向另一侧发送原始字节数据和句柄。

<!-- Two handles are returned on success, providing access to both sides
of the channel.  Messages written to one handle may be read from
the opposite. -->
调用成功则返回两个句柄，分别提供对通道两侧的访问，写入一个句柄的消息可以在相反的方向上被读取。

<!-- The handles will have *ZX_RIGHT_TRANSFER* (allowing them to be sent
to another process via channel write), *ZX_RIGHT_WRITE* (allowing
messages to be written to them), and *ZX_RIGHT_READ* (allowing messages
to be read from them). -->
返回的句柄具有*ZX_RIGHT_TRANSFER*（允许它们通过写入channel发送到另一个进程），*ZX_RIGHT_WRITE*（允许将消息写入它们）和*ZX_RIGHT_READ*（允许从它们中读取消息）权限。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **channel_create**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**channel_create()** 调用成功则返回**ZX_OK**。如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *out0* or *out1* is an invalid pointer or NULL or
*options* is any value other than 0. -->
**ZX_ERR_INVALID_ARGS**： *out0*或*out1*是无效指针或NULL，或*options*是0以外的其它任何值。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见


<!-- [handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[handle_replace](handle_replace.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[channel_call](channel_call.md),
[channel_read](channel_read.md),
[channel_write](channel_write.md). -->


[handle_close](handle_close.md)，[handle_duplicate](handle_duplicate.md)，[handle_replace](handle_replace.md)，[object_wait_one](object_wait_one.md)，[object_wait_many](object_wait_many.md)，[channel_call](channel_call.md)，[channel_read](channel_read.md)，[channel_write](channel_write.md)。

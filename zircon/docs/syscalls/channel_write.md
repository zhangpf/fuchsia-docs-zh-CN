# zx_channel_write
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/channel_write.md)

---
<!-- ## NAME -->
## 名称

<!-- channel_write - write a message to a channel -->
channel_write —— 向channel写入消息

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_channel_write(zx_handle_t handle, uint32_t options,
                             void* bytes, uint32_t num_bytes,
                             zx_handle_t* handles, uint32_t num_handles);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **channel_write**() attempts to write a message of *num_bytes*
bytes and *num_handles* handles to the channel specified by
*handle*.  The pointers *handles* and *bytes* may be NULL if their
respective sizes are zero. -->
**channel_write()** 尝试将*num_bytes*个字节和*num_handles*个句柄的消息写入*handle*指定的channel中。如果它们各自的大小为零，则*handles*和*bytes*指针可以是NULL。

<!-- On success, all *num_handles* of the handles in the *handles* array
are no longer accessible to the caller's process -- they are attached
to the message and will become available to the reader of that message
from the opposite end of the channel.  On any failure, all handles
are discarded rather than transferred. -->
执行成功时，调用者进程不再可以访问*handles*数组中所有*num_handles*个句柄——它们已加入到可以从channel的另一端读取的消息中。在任何失败发生时，所有句柄都将被丢弃而不是被传输。

<!-- 
It is invalid to include *handle* (the handle of the channel being written
to) in the *handles* array (the handles being sent in the message). -->

在（在消息中发送的句柄）*handles*数组中包含（正在写入通道的句柄本身）*handle*是无效的。

<!-- 
The maximum number of handles which may be sent in a message is
**ZX_CHANNEL_MAX_MSG_HANDLES**, which is 64. -->

可在消息中发送的最大句柄数是**ZX_CHANNEL_MAX_MSG_HANDLES**，即64。

<!-- The maximum number of bytes which may be sent in a message is
**ZX_CHANNEL_MAX_MSG_BYTES**, which is 65536. -->

可在消息中发送的最大字节数是**ZX_CHANNEL_MAX_MSG_BYTES**，即65536。

<!-- ## RIGHTS -->
## 权限

<!-- *handle* must have **ZX_RIGHT_WRITE**.

Each of the handles in *handles* must have **ZX_RIGHT_TRANSFER**. -->
*handle*必须具有*ZX_RIGHT_WRITE*权限。

*handles*中的每个句柄必须具有*ZX_RIGHT_TRANSFER*权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **channel_write**() returns **ZX_OK** on success. -->
**channel_write()** 调用成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle, any element in
*handles* is not a valid handle, or there are duplicates among the handles
in the *handles* array. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄，*handles*中存在无效句柄，或*handles*数组中的句柄存在重复项。


<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a channel handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*是无效句柄。

<!-- **ZX_ERR_INVALID_ARGS**  *bytes* is an invalid pointer, *handles*
is an invalid pointer, or *options* is nonzero. -->
**ZX_ERR_INVALID_ARGS**：提供的指针存在无效的或为null的，或*num_bytes*小于4，或者*options*为非零。

<!-- **ZX_ERR_NOT_SUPPORTED**  *handle* was found in the *handles* array, or
one of the handles in *handles* was *handle* (the handle to the
channel being written to). -->
**ZX_ERR_NOT_SUPPORTED**: *handle*在*handles*数组中被找到，或者说*handles*中的一个句柄是（正在被写入的通道句柄的）*handle*本身。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_WRITE** or
any element in *handles* does not have **ZX_RIGHT_TRANSFER**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_WRITE**权限，或*handles*中的存在没有**ZX_RIGHT_TRANSFER**权限的元素。

<!-- **ZX_ERR_PEER_CLOSED**  The other side of the channel is closed. -->
**ZX_ERR_PEER_CLOSED**：channel的另一侧已关闭。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_OUT_OF_RANGE**  *num_bytes* or *num_handles* are larger than the
largest allowable size for channel messages. -->
**ZX_ERR_OUT_OF_RANGE**： *num_bytes*或*num_handles*大于channel最大允许的消息大小。

<!-- ## NOTES -->
## 注意

<!-- 
*num_handles* is a count of the number of elements in the *handles*
array, not its size in bytes. -->
*num_handles*是*handles*数组中元素个数的计数，而不是以字节为单位的大小。

<!-- The byte size limitation on messages is not yet finalized. -->
消息的字节大小限制尚未最终确定。

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md)

[handle_duplicate](handle_duplicate.md)

[handle_replace](handle_replace.md)

[object_wait_one](object_wait_one.md)

[object_wait_many](object_wait_many.md)

[channel_call](channel_call.md)

[channel_create](channel_create.md)

[channel_read](channel_read.md)

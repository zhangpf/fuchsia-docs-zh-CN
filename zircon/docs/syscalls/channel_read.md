# zx_channel_read  - zx_channel_read_etc
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/channel_read.md)

---
<!-- ## NAME -->
## 名称

<!-- channel_read - read a message from a channel -->
channel_read —— 向channel写入消息

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_channel_read(zx_handle_t handle, uint32_t options,
                            void* bytes, zx_handle_t* handles,
                            uint32_t num_bytes, uint32_t num_handles,
                            uint32_t* actual_bytes, uint32_t* actual_handles);


zx_status_t zx_channel_read_etc(zx_handle_t handle, uint32_t options,
                                void* bytes, zx_handle_info_t* handles,
                                uint32_t num_bytes, uint32_t num_handles,
                                uint32_t* actual_bytes, uint32_t* actual_handles);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **channel_read**() and **channel_read_etc**() attempts to read the first
message from the channel specified by *handle* into the provided *bytes*
and/or *handles* buffers. -->
**channel_read()**和**channel_read_etc()** 尝试将句柄指定的channel中的第一条消息读入到提供的*bytes*和/或*handles*缓冲区中。

<!-- The parameters *num_bytes* and *num_handles* are used to specify the
size of the respective read buffers. -->
参数*num_bytes*和*num_handles*用于指定相应读缓冲区的大小。
<!-- 
Channel messages may contain both byte data and handle payloads and may
only be read in their entirety.  Partial reads are not possible. -->
channel消息可以包含字节数据和句柄有效载荷，并且仅可以完整地被读取，而部分读取是不可能的。

<!-- The *bytes* buffer is written before the *handles* buffer. In the event of
overlap between these two buffers, the contents written to *handles*
will overwrite the portion of *bytes* it overlaps. -->
*bytes*缓冲区在*handles*缓冲区之前被写入。如果这两个缓冲区重叠，写入*handles*的内容将覆盖掉重叠的*bytes*部分。

<!-- Both forms of read behave the same except that **channel_read**() returns an
array of raw ``zx_handle_t`` handle values while **channel_read_etc**() returns
an array of ``zx_handle_info_t`` structures of the form: -->
两种形式的读取行为相同，只是**channel_read()** 返回原始``zx_handle_t``句柄值的数组，而**channel_read_etc()** 返回``zx_handle_info_t``形式的结构体数组：

<!-- ```
typedef struct {
    zx_handle_t handle;     // handle value
    zx_obj_type_t type;     // type of object, see ZX_OBJ_TYPE_
    zx_rights_t rights;     // handle rights
    uint32_t unused;        // set to zero
} zx_handle_info_t;
``` -->
```
typedef struct {
    zx_handle_t handle;     // handle值
    zx_obj_type_t type;     // 对象类型，参考ZX_OBJ_TYPE_
    zx_rights_t rights;     // handle权限
    uint32_t unused;        // 置为0
} zx_handle_info_t;
```
<!-- When communicating to an untrusted party over a channel, it is recommended
that the **channel_read_etc**() form is used and each handle type and rights
are validated against the expected values. -->
通过channel与不受信任方通信时，建议使用**channel_read_etc()** 形式，并根据预期值验证每个返回句柄的类型和权限。

<!-- ## RIGHTS -->
## 权限

<!-- *handle* must have **ZX_RIGHT_READ**. -->
*handle*必须具有**ZX_RIGHT_READ**权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- Both forms of read returns **ZX_OK** on success, if *actual_bytes*
and *actual_handles* (if non-NULL), contain the exact number of bytes
and count of handles read. -->
两种形式的读取，成功皆返回**ZX_OK**，并且*actual_bytes*和*actual_handles*（如果非NULL）包含准确的读取字节数的句柄数量。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a channel handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是channel类型句柄。

<!-- **ZX_ERR_INVALID_ARGS**  If any of *bytes*, *handles*, *actual_bytes*, or
*actual_handles* are non-NULL and an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**  任何*bytes*，*handles*，*actual_bytes*或*actual_handles*之一是非NULL的无效指针。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_READ**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*不具有**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_SHOULD_WAIT**  The channel contained no messages to read. -->
**ZX_ERR_SHOULD_WAIT**: channel不包含可读的消息。


<!-- **ZX_ERR_PEER_CLOSED**  The other side of the channel is closed. -->
**ZX_ERR_PEER_CLOSED**：channel的另一侧已关闭。


<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_BUFFER_TOO_SMALL**  The provided *bytes* or *handles* buffers
are too small (in which case, the minimum sizes necessary to receive
the message will be written to *actual_bytes* and *actual_handles*,
provided they are non-NULL). If *options* has **ZX_CHANNEL_READ_MAY_DISCARD**
set, then the message is discarded. -->
**ZX_ERR_BUFFER_TOO_SMALL**：提供的*bytes*或*handles*缓冲区太小（在这种情况下，接收消息所需的最小大小将写入*actual_bytes*和*actual_handles*中，前提是它们为非NULL）。如果*options*设置了**ZX_CHANNEL_READ_MAY_DISCARD**选项，则丢弃该消息。

<!-- ## NOTES -->
## 注释

<!-- *num_handles* and *actual_handles* are counts of the number of elements
in the *handles* array, not its size in bytes. -->
*num_handles*和*actual_handles*是*handles*数组中元素个数的计数，而不是其（以字节为单位）的大小。

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md)

[handle_duplicate](handle_duplicate.md)

[handle_replace](handle_replace.md)

[object_wait_one](object_wait_one.md)

[object_wait_many](object_wait_many.md)

[channel_call](channel_call.md)

[channel_create](channel_create.md)

[channel_write](channel_write.md)

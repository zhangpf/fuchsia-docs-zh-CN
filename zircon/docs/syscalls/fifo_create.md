# zx_fifo_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/fifo_create.md)

---
<!-- ## NAME -->
## 名称

<!-- fifo_create - create a fifo -->
fifo_create —— 创建fifo

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_fifo_create(size_t elem_count, size_t elem_size,
                           uint32_t options,
                           zx_handle_t* out0, zx_handle_t* out1);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **fifo_create**() creates a fifo, which is actually a pair of fifos
of *elem_count* entries of *elem_size* bytes.  Two endpoints are
returned.  Writing to one endpoint enqueus an element into the fifo
that the opposing endpoint reads from. -->

**fifo_create()** 创建一个fifo，它实际上是*elem_count*项，每个项有*elem_size*字节的一对fifo。调用的结果是返回两个端点(endpoint)。从一个端点写入数据的结果，会将一个元素加入到相对端点(opposing endpoint)能够读取的fifo中。

<!-- Fifos are intended to be the control plane for shared memory transports.
Their read and write operations are more efficient than *sockets* or
*channels*, but there are severe restrictions on the size of elements
and buffers. -->
fifo旨在成为共享内存传输的控制面。它们的读写操作比*socket*或*channel*更高效，但是对元素和缓冲区的大小有严格的限制。

<!-- The *elem_count* must be a power of two.  The total size of each fifo
(*elem_count* * *elem_size*) may not exceed 4096 bytes. -->
*elem_count*大小必须是2的幂，每个fifo的总大小(*elem_count* * *elem_size*)不得超过4096个字节。

<!-- The *options* argument must be 0. -->
*options*参数必须为0。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值
<!-- 
**fifo_create**() returns **ZX_OK** on success. In the event of
failure, one of the following values is returned. -->


**fifo_create()** 函数执行成功则返回 **ZX_OK**，如果发生错误，将返回以下值之一。

<!-- ## ERRORS -->

## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *out0* or *out1* is an invalid pointer or NULL or
*options* is any value other than 0. -->

**ZX_ERR_INVALID_ARGS**： *out0*或*out1*是无效指针或NULL，或者*options*是0以外的其它任何值。

<!-- **ZX_ERR_OUT_OF_RANGE**  *elem_count* or *elem_size* is zero, or *elem_count*
is not a power of two, or *elem_count* * *elem_size* is greater than 4096. -->
**ZX_ERR_OUT_OF_RANGE**： 可能是下列情况之一，*elem_count*或*elem_size*为零；*elem_count*不是2的幂；*elem_count* * *elem_size*大于4096。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->

**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

<!-- [fifo_read](fifo_read.md),
[fifo_write](fifo_write.md). -->

[fifo_read](fifo_read.md)，[fifo_write](fifo_write.md)。

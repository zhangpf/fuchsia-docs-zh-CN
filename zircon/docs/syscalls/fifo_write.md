# zx_fifo_write
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/fifo_write.md)

---
<!-- ## NAME -->
## 名称

<!-- fifo_write - write data to a fifo -->
fifo_write —— 写入数据到fifo中

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_fifo_write(zx_handle_t handle, size_t elem_size,
                          const void* buffer, size_t count, size_t* actual_count);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **fifo_write**() attempts to write up to *count* elements
(*count * elem_size* bytes) from *buf* to the fifo specified by *handle*. -->
**fifo_write()** 尝试从*buf*向*handle*指定的fifo写入*count*个元素（即*count * elem_size*个字节）。

<!-- Fewer elements may be written than requested if there is insufficient
room in the fifo to contain all of them. The number of
elements actually written is returned via *actual_count*. -->
如果fifo中没有足够的空间来容纳所有元素，则写入比请求更少数量的元素，实际写入的元素数量通过*actual_count*返回。

<!-- The element size specified by *elem_size* must match the element size
that was passed into **fifo_create**(). -->
*elem_size*指定的元素大小必须与传给 **fifo_create()** 的元素大小一致。

<!-- *actual_count* is allowed to be NULL. This is useful when writing
a single element: if *count* is 1 and **fifo_write**() returns **ZX_OK**,
*actual_count* is guaranteed to be 1 and thus can be safely ignored. -->
*actual_count*允许为NULL，这对于写入单个元素时很有用：如果*count*为1，且**fifo_write()** 返回 **ZX_OK**，那么*actual_count*将保证为1，因此可以安全地被忽略掉。

<!-- It is not legal to write zero elements. -->
写入零个元素是非法的。

<!-- ## RIGHTS -->
## 权限

<!-- *handle* must have **ZX_RIGHT_WRITE**. -->
*handle*必须具有**ZX_RIGHT_WRITE**权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **fifo_write**() returns **ZX_OK** on success, and returns
the number of elements written (at least one) via *actual_count*. -->
**fifo_write()** 调用成功则返回 **ZX_OK**，并通过*actual_count*返回写入的元素数量（至少为1）。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle.

**ZX_ERR_WRONG_TYPE**  *handle* is not a fifo handle.

**ZX_ERR_INVALID_ARGS**  *buffer* is an invalid pointer or *actual_count*
is an invalid pointer.

**ZX_ERR_OUT_OF_RANGE**  *count* is zero or *elem_size* is not equal
to the element size of the fifo.

**ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_WRITE**.

**ZX_ERR_PEER_CLOSED**  The other side of the fifo is closed.

**ZX_ERR_SHOULD_WAIT**  The fifo is full. -->

**ZX_ERR_BAD_HANDLE**： *handle*是无效句柄。

**ZX_ERR_WRONG_TYPE**： *handle*不是fifo类型句柄。

**ZX_ERR_INVALID_ARGS**： *buffer*是无效指针或*actual_count*是无效指针。

**ZX_ERR_OUT_OF_RANGE**： *count*为零或*elem_size*不等于fifo的元素大小。

**ZX_ERR_ACCESS_DENIED**： *handle*没有**ZX_RIGHT_WRITE**权限。

**ZX_ERR_PEER_CLOSED**： fifo的另一侧已关闭。

**ZX_ERR_SHOULD_WAIT**： fifo已满。

<!-- ## SEE ALSO -->
## 另见

[fifo_create](fifo_create.md)

[fifo_read](fifo_read.md)
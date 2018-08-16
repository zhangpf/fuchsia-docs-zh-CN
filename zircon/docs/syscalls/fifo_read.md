# zx_fifo_read
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/fifo_read.md)

---
<!-- ## NAME -->
## 名称

<!-- fifo_read - read data from a fifo -->
fifo_read —— 从fifo中读取数据

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_fifo_read(zx_handle_t handle, size_t elem_size,
                         void* buffer, size_t count, size_t* actual_count);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **fifo_read**() attempts to read up to *count* elements from the fifo
*handle* into *buffer*. -->
**fifo_read()** 尝试从*handle*指向的fifo中读取*count*个元素到*buffer*。

<!-- Fewer elements may be read than requested if there are insufficient
elements in the fifo to fulfill the entire request. The number of
elements actually read is returned via *actual_count*. -->
如果fifo中没有足够的元素来满足整个请求，则读取比请求的元素少的数量，实际读取的元素数量通过*actual_count*返回。

<!-- The element size specified by *elem_size* must match the element size
that was passed into **fifo_create**(). -->
*elem_size*指定的元素大小必须与传递给 **fifo_create()** 的元素大小一致。

<!-- *buffer* must have a size of at least *count * elem_size* bytes. -->
*buffer*的大小必须至少为*count* * *elem_size*个字节。
 
<!-- *actual_count* is allowed to be NULL. This is useful when reading
a single element: if *count* is 1 and **fifo_read**() returns **ZX_OK**,
*actual_count* is guaranteed to be 1 and thus can be safely ignored. -->
*actual_count*允许为NULL，这对于读取单个元素时很有用：如果*count*为1且 **fifo_read()** 返回 **ZX_OK**，那么*actual_count*保证为1，因此可以安全地被忽略。

<!-- It is not legal to read zero elements. -->
读取零个元素是非法的。

<!-- ## RIGHTS -->
## 权限

<!-- *handle* must have **ZX_RIGHT_READ**. -->
*handle*必须具有**ZX_RIGHT_READ**权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **fifo_read**() returns **ZX_OK** on success, and returns
the number of elements read (at least one) via *actual_count*. -->
**fifo_read()** 调用成功则返回 **ZX_OK**，并通过*actual_count*返回读取的元素数量（至少为1）。


<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle.

**ZX_ERR_WRONG_TYPE**  *handle* is not a fifo handle.

**ZX_ERR_INVALID_ARGS**  *buffer* is an invalid pointer or *actual_count*
is an invalid pointer.

**ZX_ERR_OUT_OF_RANGE**  *count* is zero or *elem_size* is not equal
to the element size of the fifo.

**ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_READ**.

**ZX_ERR_PEER_CLOSED**  The other side of the fifo is closed.

**ZX_ERR_SHOULD_WAIT**  The fifo is empty. -->

**ZX_ERR_BAD_HANDLE**： *handle*是无效句柄。

**ZX_ERR_WRONG_TYPE**： *handle*不是fifo类型句柄。

**ZX_ERR_INVALID_ARGS**： *buffer*是无效指针或*actual_count*是无效指针。

**ZX_ERR_OUT_OF_RANGE**： *count*为零或*elem_size*不等于fifo的元素大小。

**ZX_ERR_ACCESS_DENIED**： *handle*没有**ZX_RIGHT_READ**权限。

**ZX_ERR_PEER_CLOSED**： fifo的另一侧已关闭。

**ZX_ERR_SHOULD_WAIT**： fifo已满。

<!-- ## SEE ALSO -->
## 另见

<!-- [fifo_create](fifo_create.md),
[fifo_write](fifo_write.md). -->

[fifo_create](fifo_create.md)，[fifo_write](fifo_write.md)。

# zx_vmo_write
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_write.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_write - write bytes to the VMO -->
vmo_write —— 向VMO写入字节

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_write(zx_handle_t handle, const void* buffer,
                         uint64_t offset, size_t buffer_size);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmo_write**() attempts to write exactly *buffer_size* bytes to a VMO at *offset*. -->
**vmo_write()** 的功能时试图将*buffer_size*字节写入VMO的*offset*位移处。
<!-- 
*buffer* pointer to a user buffer to write bytes from. -->
*buffer*是指向用户缓冲区的指针，用于字节写入。

<!-- *buffer_size* number of bytes to attempt to write. -->
*buffer_size*是试图写入的字节数。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_vmo_write**() returns **ZX_OK** on success, and exactly *buffer_size* bytes will
have been written from *buffer*.
In the event of failure, a negative error value is returned, and the number of
bytes written from *buffer* is undefined. -->
**zx_vmo_write()** 调用成功则返回**ZX_OK**，并且将写入从*buffer*读取的*buffer_size*字节的数据。
如果发生错误，则返回负的错误码，且写入的字节数是不确定的。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a VMO handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是VMO类型的句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have the **ZX_RIGHT_WRITE** right. -->
**ZX_ERR_ACCESS_DENIED**：*handle*不具有**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_INVALID_ARGS**  *buffer* is an invalid pointer or NULL. -->
**ZX_ERR_INVALID_ARGS**：*buffer*是无效指针或`NULL`。

<!-- **ZX_ERR_NO_MEMORY**  Failure to allocate system memory to complete write. -->
**ZX_ERR_NO_MEMORY**：无法分配足够系统内存以完成写入操作。

<!-- **ZX_ERR_OUT_OF_RANGE**  *offset* starts at or beyond the end of the VMO, or VMO is shorter than *buffer_size*. -->
**ZX_ERR_OUT_OF_RANGE**：*offset*大于或等于VMO结束位置，或者VMO大小小于*buffer_size*。

<!-- **ZX_ERR_BAD_STATE**  VMO has been marked uncached and is not directly writable. -->
**ZX_ERR_BAD_STATE**：VMO已标记为未缓存，无法直接写入。

<!-- ## SEE ALSO -->
## 另见

[vmo_create](vmo_create.md),
[vmo_clone](vmo_clone.md),
[vmo_read](vmo_read.md),
[vmo_get_size](vmo_get_size.md),
[vmo_set_size](vmo_set_size.md),
[vmo_op_range](vmo_op_range.md).
[vmo_set_cache_policy](vmo_set_cache_policy.md)
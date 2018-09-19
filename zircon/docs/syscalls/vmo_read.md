# zx_vmo_read
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_read.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_read - read bytes from the VMO -->
vmo_read —— 从VMO中读取字节

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_read(zx_handle_t handle, void* buffer, uint64_t offset, size_t buffer_size);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmo_read**() attempts to read exactly *buffer_size* bytes from a VMO at *offset*. -->
**vmo_write()** 的功能是试图从VMO的*offset*位移处读取*buffer_size*字节的数据。

<!-- *buffer* pointer to a user buffer to read bytes into. -->
*buffer*是指向用户缓冲区的指针，用于读入字节。

<!-- *buffer_size* number of bytes to attempt to read. *buffer* buffer should be large
enough for at least this many bytes. -->
*buffer_size*是试图读取的字节数，并且*buffer*缓冲区需即至少有*buffer_size*字节。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_vmo_read**() returns **ZX_OK** on success, and exactly *buffer_size* bytes will
have been written to *buffer*.
In the event of failure, a negative error value is returned, and the number of
bytes written to *buffer* is undefined. -->
**zx_vmo_read()** 调用成功则返回**ZX_OK**，并且将*buffer_size*字节的数据写入到*buffer*中。
如果发生错误，则返回负的错误码，且写入*buffer*的字节数是不确定的。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a VMO handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是VMO类型的句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have the **ZX_RIGHT_READ** right. -->
**ZX_ERR_ACCESS_DENIED**：*handle*不具有**ZX_RIGHT_READ**权限。

<!-- **ZX_ERR_INVALID_ARGS**  *buffer* is an invalid pointer or NULL. -->
**ZX_ERR_INVALID_ARGS**：*buffer*是无效指针或`NULL`。

<!-- **ZX_ERR_OUT_OF_RANGE**  *offset* starts at or beyond the end of the VMO,
                         or VMO is shorter than *buffer_size*. -->
**ZX_ERR_OUT_OF_RANGE**：*offset*大于或等于VMO的结束位置，或者VMO大小小于*buffer_size*。

<!-- **ZX_ERR_BAD_STATE**  VMO has been marked uncached and is not directly readable. -->
**ZX_ERR_BAD_STATE**：VMO已标记为未缓存，无法直接读取。

<!-- ## SEE ALSO -->
## 另见

[vmo_create](vmo_create.md),
[vmo_clone](vmo_clone.md),
[vmo_write](vmo_write.md),
[vmo_get_size](vmo_get_size.md),
[vmo_set_size](vmo_set_size.md),
[vmo_op_range](vmo_op_range.md).
[vmo_set_cache_policy](vmo_set_cache_policy.md)
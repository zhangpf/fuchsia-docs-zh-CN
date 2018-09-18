# zx_vmo_get_size
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_get_size.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_get_size - read the current size of a VMO object -->
vmo_get_size —— 获取VMO对象当前的大小

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_get_size(zx_handle_t handle, uint64_t* size);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmo_get_size**() returns the current size of the VMO. -->
**vmo_get_size()** 返回VMO当前的大小。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmo_get_size**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**vmo_get_size()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a VMO handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是VMO类型句柄。

<!-- **ZX_ERR_INVALID_ARGS**  *size* is an invalid pointer or NULL. -->
**ZX_ERR_INVALID_ARGS**：*size*是无效指针或为`NULL`。

<!-- ## SEE ALSO -->
## 另见

[vmo_create](vmo_create.md),
[vmo_clone](vmo_clone.md),
[vmo_read](vmo_read.md),
[vmo_write](vmo_write.md),
[vmo_set_size](vmo_set_size.md),
[vmo_op_range](vmo_op_range.md).
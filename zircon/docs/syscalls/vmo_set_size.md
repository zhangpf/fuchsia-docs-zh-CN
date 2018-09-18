# zx_vmo_set_size
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_set_size.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_set_size - resize a VMO object -->
vmo_get_size —— 调整VMO对象的大小

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_set_size(zx_handle_t handle, uint64_t size);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmo_set_size**() sets the new size of a VMO object. -->
**vmo_set_size()** 的功能是设置VMO对象新的大小。

<!-- The size will be rounded up to the next page size boundary.
Subsequent calls to **vmo_get_size**() will return the rounded up size. -->
实际大小将向上舍入到下一页边界位置的代销。 
对**vmo_get_size()** 的后续调用将返回此向上舍入后的大小值。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmo_set_size**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**vmo_set_size()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a VMO handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是VMO类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have the **ZX_RIGHT_WRITE** right. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_UNAVAILABLE** The VMO was created with **ZX_VMO_NON_RESIZABLE** option. -->
**ZX_ERR_UNAVAILABLE**：VMO是使用**ZX_VMO_NON_RESIZABLE**标志位创建的对象，因而无法更改大小。

<!-- **ZX_ERR_OUT_OF_RANGE**  Requested size is too large. -->
**ZX_ERR_OUT_OF_RANGE**：请求调整的大小太大。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of system memory. -->
**ZX_ERR_NO_MEMORY**：由于缺少系统内存而导致的失败。

<!-- ## SEE ALSO -->
## 另见

[vmo_create](vmo_create.md),
[vmo_clone](vmo_clone.md),
[vmo_read](vmo_read.md),
[vmo_write](vmo_write.md),
[vmo_get_size](vmo_get_size.md),
[vmo_op_range](vmo_op_range.md).
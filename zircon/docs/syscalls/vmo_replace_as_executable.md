# zx_vmo_replace_as_executable
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_replace_as_executable.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_replace_as_executable - add execute rights to a vmo -->
vmo_replace_as_executable —— 向vmo添加可执行权限

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_replace_as_executable(zx_handle_t vmo, zx_handle_t vmex, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmo_replace_as_executable**() creates a replacement for *vmo*, referring
to the same underlying VM object, adding the right **ZX_RIGHT_EXECUTE**. -->
**vmo_replace_as_executable()** 的功能是使用新的虚拟内存对象替换*vmo*，它引用与*vmo*相同的底层VM对象，并添加**ZX_RIGHT_EXECUTE**权限。

<!-- *vmo* is always invalidated. -->
操作完成后*vmo*将失效。

<!-- ## RIGHTS -->
## 权限

<!-- *vmex* must be a valid **ZX_RSRC_KIND_VMEX** resource handle,
or **ZX_HANDLE_INVALID** (to ease migration of old code). -->
*vmex*必须是有效的**ZX_RSRC_KIND_VMEX**类型资源句柄，或**ZX_HANDLE_INVALID**（以便于迁移旧代码）。

<!-- TODO(SEC-42): Disallow **ZX_HANDLE_INVALID**. -->
TODO(SEC-42)：禁止使用**ZX_HANDLE_INVALID**。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmo_replace_as_executable**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**vmo_replace_as_executable()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *vmo* isn't a valid VM object handle, or
*vmex* isn't a valid **ZX_RSRC_KIND_VMEX** resource handle. -->
**ZX_ERR_BAD_HANDLE**：*vmo*不是有效的VM对象句柄，或*vmex*不是有效的**ZX_RSRC_KIND_VMEX**资源句柄。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[resource_create](resource_create.md),
[vmar_map](vmar_map.md).
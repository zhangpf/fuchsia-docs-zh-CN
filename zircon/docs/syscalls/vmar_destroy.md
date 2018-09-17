# zx_vmar_destroy
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmar_destroy.md)

---
<!-- ## NAME -->
## 名称

<!-- vmar_destroy - destroy a virtual memory address region -->
vmar_destroy —— 销毁虚拟内存地址区域
<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmar_destroy(zx_handle_t vmar_handle);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmar_destroy**() unmaps all mappings within the given region, and destroys
all sub-regions of the region.  Note that this operation is logically recursive. -->
**vmar_destroy()** 的功能是取消给定区域内的所有映射，并销毁该区域的所有子区域。
注意，此操作在逻辑上是递归的。

<!-- This operation does not close *vmar_handle*.  Any outstanding handles to this
VMAR will remain valid handles, but all VMAR operations on them will fail. -->
此操作不会关闭*vmar_handle*，指向此VMAR的任何未关闭的句柄仍将是有效句柄，但它们的所有对VMAR的操作都将失败。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmar_destroy**() returns **ZX_OK** on success. -->
**vmar_destroy()** 调用成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *vmar_handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*vmar_handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *vmar_handle* is not a VMAR handle. -->
**ZX_ERR_WRONG_TYPE**：*vmar_handle*不是VMAR类型的句柄。

<!-- 
**ZX_ERR_BAD_STATE**  This region is already destroyed. -->
**ZX_ERR_BAD_STATE**：此区域已被销毁。

<!-- ## NOTES -->
## 注释

<!-- ## SEE ALSO -->
## 另见

[vmar_allocate](vmar_allocate.md),
[vmar_map](vmar_map.md),
[vmar_protect](vmar_protect.md),
[vmar_unmap](vmar_unmap.md).
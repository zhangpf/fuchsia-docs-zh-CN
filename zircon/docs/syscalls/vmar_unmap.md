# zx_vmar_unmap
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmar_unmap.md)

---
<!-- ## NAME -->
## 名称

<!-- vmar_unmap - unmap virtual memory pages -->
vmar_unmap —— 取消映射虚拟内存页面

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmar_unmap(zx_handle_t handle, zx_vaddr_t addr, uint64_t len);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmar_unmap**() unmaps all VMO mappings and destroys (as if **vmar_destroy**
were called) all sub-regions within the absolute range including *addr* and ending
before exclusively at *addr* + *len*.  Any sub-region that is in the range must
be fully in the range (i.e. partial overlaps are an error).  If a mapping is
only partially in the range, the mapping is split and the requested portion is
unmapped. -->
**vmar_unmap()** 取消所有VMO的映射并销毁（如同调用**vmar_destroy**一样）在`[*addr*, *addr* + *len*)`范围内的所有子区域。 
范围内的任何子区域必须完全在该范围内（即部分重叠会导致错误）。 
如果内存映射仅部分在该范围内，则映射将被拆分，并且请求范围内的部分将被取消映射。

<!-- *len* must be page-aligned. -->
*len*的大小必须是页面对齐的。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmar_unmap**() returns **ZX_OK** on success. -->
**vmar_unmap()** 调用成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *vmar_handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*vmar_handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *vmar_handle* is not a VMAR handle. -->
**ZX_ERR_WRONG_TYPE**：*vmar_handle*不是VMAR类型的句柄。

<!-- **ZX_ERR_INVALID_ARGS**  *addr* is not page-aligned, *len* is 0 or not page-aligned,
or the requested range partially overlaps a sub-region. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*addr*不是页面对齐；*len*是0或不是页面对齐；或者请求的范围部分地与子区域重叠。

<!-- **ZX_ERR_BAD_STATE**  *vmar_handle* refers to a destroyed handle. -->
**ZX_ERR_BAD_STATE**：*vmar_handle*句柄指向的是已销毁对象。

<!-- **ZX_ERR_NOT_FOUND**  Could not find the requested mapping. -->
**ZX_ERR_NOT_FOUND**：找不到请求的映射。

<!-- ## NOTES -->
## 注释

<!-- ## SEE ALSO -->
## 另见

[vmar_allocate](vmar_allocate.md),
[vmar_destroy](vmar_destroy.md),
[vmar_map](vmar_map.md),
[vmar_protect](vmar_protect.md).
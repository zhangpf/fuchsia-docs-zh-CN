# zx_bti_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fc9cc3958ceaacca9447023b8b272f6b648f75ee/docs/syscalls/bti_create.md)

---
<!-- ## NAME -->
## 名称

<!-- bti_create - create a new bus transaction initiator -->
bti_create —— 创建新的总线事务启动器

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_bti_create(zx_handle_t iommu, uint32_t options, uint64_t bti_id, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **bti_create**() creates a new [bus transaction initiator](../objects/bus_transaction_initiator.md)
given a handle to an IOMMU and a hardware transaction identifier for a device
downstream of that IOMMU. -->
**bti_create()** 创建一个新的[总线事务启动器](../objects/bus_transaction_initiator.md)，给定IOMMU的句柄和该IOMMU下游设备的硬件事务标识符。

<!-- *options* must be 0 (reserved for future definition of creation flags). -->
*options*必须为0（保留用作将来创建标识位的定义）。

<!-- Upon success a handle for the new BTI is returned.  This handle will have rights
**ZX_RIGHT_READ**, **ZX_RIGHT_WRITE**, **ZX_RIGHT_MAP**, **ZX_RIGHT_INSPECT**,
**ZX_RIGHT_DUPLICATE**, and **ZX_RIGHT_TRANSFER**. -->
调用成功后，将返回新BTI的句柄。
该句柄将具有**ZX_RIGHT_READ**，**ZX_RIGHT_WRITE**，**ZX_RIGHT_MAP**，**ZX_RIGHT_INSPECT**，**ZX_RIGHT_DUPLICATE**和**ZX_RIGHT_TRANSFER**权限。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **bti_create**() returns **ZX_OK** and a handle to the new BTI
(via *out*) on success.  In the event of failure, a negative error value
is returned. -->
**bti_create()** 调用成功则返回**ZX_OK**和（通过*out*）返回新BTI句柄。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *iommu* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*iommu*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**：  *iommu* is not an iommu handle. -->
**ZX_ERR_WRONG_TYPE**：*iommu*不是iommu类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *iommu* handle does not have sufficient privileges. -->
**ZX_ERR_ACCESS_DENIED**：*iommu*句柄没有足够的权限。
<!-- 
**ZX_ERR_INVALID_ARGS**: *bti_id* is invalid on the given IOMMU,
*out* is an invalid pointer, or *options* is non-zero. -->
**ZX_ERR_INVALID_ARGS**：*bti_id*在给定的IOMMU上无效，或*out*是无效指针，或*options*非零。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[bti_pin](bti_pin.md),
[bti_release_quarantine](bti_release_quarantine.md)
[pmt_unpin](pmt_unpin.md).

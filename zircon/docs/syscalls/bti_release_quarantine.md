# zx_bti_release_quarantine
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fc9cc3958ceaacca9447023b8b272f6b648f75ee/docs/syscalls/bti_release_quarantine.md)

---
<!-- ## NAME -->
## 名称

<!-- bti_release_quarantine - releases all quarantined PMTs -->
bti_release_quarantine —— 释放所有隔离的PMT

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_bti_release_quarantine(zx_handle_t bti);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **bti_release_quarantine**() releases all quarantined PMTs for the given BTI.
This will release the PMTs' underlying references to VMOs and physical page
pins.  The underlying physical pages may be eligible to be reallocated
afterwards. -->
**bti_release_quarantine()** 释放给定BTI的所有隔离PMT，
这将同时释放PMT对VMO和物理页面固定的底层引用，底层物理页面可在此调用后被释放并重新分配。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **bti_release_quarantine**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->

**bti_release_quarantine()** 调用成功返回**ZX_OK**。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *bti* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*bti*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *bti* is not a BTI handle. -->
**ZX_ERR_WRONG_TYPE**：*bti*不是BTI类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED** *bti* does not have the *ZX_RIGHT_WRITE* right. -->
**ZX_ERR_ACCESS_DENIED**：*bti*没有*ZX_RIGHT_WRITE*权限。

<!-- ## SEE ALSO -->
## 另见

[bti_pin](bti_pin.md),
[pmt_unpin](pmt_unpin.md)
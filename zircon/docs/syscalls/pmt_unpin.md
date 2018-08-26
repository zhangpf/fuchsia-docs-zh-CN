# zx_pmt_unpin
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/daec3be241ed2d5dbdf2ed91ee3ac469a7c72fae/docs/syscalls/pmt_unpin.md)

---
<!-- ## NAME -->
## 名称

<!-- pmt_unpin - unpin pages and revoke device access to them -->
pmt_unpin —— 取消固定内存页面，并撤消设备对它们的访问权限

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_pmt_unpin(zx_handle_t pmt);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **pmt_unpin**() unpins pages that were previously pinned by **bti_pin**(),
and revokes the access that was granted by the pin call. -->
**pmt_unpin()** 取消先前由**bti_pin()** 固定的页面，并撤消bti_pin授予的访问权限。

<!-- Always consumes the handle *pmt*. It is invalid to use *pmt* afterwards,
including to call **handle_close**() on it. -->
调用总是会销毁*pmt*句柄，之后再使用*pmt*将是无效的，包括在其上调用**handle_close()**。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- On success, **pmt_unpin**() returns *ZX_OK*.
In the event of failure, a negative error value is returned. -->
调用成功，**pmt_unpin()** 返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *pmt* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*pmt*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *pmt* is not a PMT handle. -->
**ZX_ERR_WRONG_TYPE**：*pmt*不是PMT类型句柄。

<!-- ## SEE ALSO -->
## 另见

[bti_create](bti_create.md),
[bti_release_quarantine](bti_release_quarantine.md),
[bti_pin](bti_pin.md).
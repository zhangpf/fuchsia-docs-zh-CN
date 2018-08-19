# zx_handle_duplicate
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/handle_duplicate.md)

---
<!-- ## NAME -->
## 名称

<!-- handle_duplicate - duplicate a handle -->
handle_duplicate —— 复制句柄

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_handle_duplicate(zx_handle_t handle, zx_rights_t rights, zx_handle_t* out);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **handle_duplicate**() creates a duplicate of *handle*, referring
to the same underlying object, with new access rights *rights*. -->
**handle_duplicate()** 创建*handle*的副本，并引用相同的底层对象，以及具有新的访问权限*rights*。

<!-- To duplicate the handle with the same rights use **ZX_RIGHT_SAME_RIGHTS**. If different
rights are desired they must be strictly lesser than of the source handle. It is possible
to specify no rights by using 0. -->
为了与原始句柄具有相同的权限，请使用**ZX_RIGHT_SAME_RIGHTS**。如果需要不同的权限，那么它们必须严格是原始句柄权限的子集。可以使用0来指定无权限的句柄。

<!-- ## RIGHTS -->
## 权限

<!-- *handle* must have **ZX_RIGHT_DUPLICATE**. -->
*handle*必须具有**ZX_RIGHT_DUPLICATE**权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **handle_duplicate**() returns ZX_OK and the duplicate handle via *out* on success. -->
**handle_duplicate()** 执行成功，则返回ZX_OK，并通过*out*返回复制后的句柄。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* isn't a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_INVALID_ARGS**  The *rights* requested are not a subset of *handle* rights or
*out* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：请求的权限*rights*不是*handle*权限的子集，或*out*是无效指针。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_DUPLICATE** and may not be duplicated. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_DUPLICATE**权限，即不可被复制。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

<!-- [handle_close](handle_close.md),
[handle_close_many](handle_close_many.md),
[handle_replace](handle_replace.md),
[rights](../rights.md). -->

[handle_close](handle_close.md)，[handle_close_many](handle_close_many.md)，[handle_replace](handle_replace.md)，[rights（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/rights.md)。
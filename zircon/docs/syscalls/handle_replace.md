# zx_handle_replace
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/handle_replace.md)

---
<!-- ## NAME -->
## 名称

<!-- handle_replace - replace a handle -->
handle_replace —— 替换句柄

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_handle_replace(zx_handle_t handle, zx_rights_t rights, zx_handle_t* out);
```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**handle_replace**() creates a replacement for *handle*, referring to
the same underlying object, with new access rights *rights*. -->

**handle_replace()** 为*handle*创建一个引用了相同底层对象，并且具有新的访问权限*rights*的替换。

<!-- *handle* is always invalidated. -->


替换之后，*handle*将作废掉。

<!-- If *rights* is **ZX_RIGHT_SAME_RIGHTS**, the replacement handle will
have the same rights as the original handle. Otherwise, *rights* must be
a subset of original handle's rights. -->

如果*rights*是**ZX_RIGHT_SAME_RIGHTS**权限，则替换句柄将具有与原始句柄相同的权限。否则，*rights*必须是原始句柄权限的子集。

<!-- ## RIGHTS -->
## 权限

<!-- No rights are required. -->
无需任何权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **handle_replace**() returns ZX_OK and the replacement handle (via *out)
on success. -->
**handle_replace()** 成功则返回ZX_OK，以及（通过*out传递）替换后的句柄。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* isn't a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_INVALID_ARGS**  The *rights* requested are not a subset of
*handle*'s rights or *out* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*rights*请求不是*handle*权限的子集，或者*out*是无效指针。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

<!-- [handle_close](handle_close.md),
[handle_close_many](handle_close_many.md),
[handle_duplicate](handle_duplicate.md). -->

[handle_close](handle_close.md)，[handle_close_many](handle_close_many.md)，[handle_duplicate](handle_duplicate.md)。
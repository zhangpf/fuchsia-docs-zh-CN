# zx_handle_close_many
---
[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/handle_close_many.md)

---
<!-- ## NAME -->
## 名称

<!-- handle_close_many - close a number of handles -->
handle_close_many —— 关闭多个句柄

<!-- ## SYNOPSIS -->
## 概览

```
#include <zircon/syscalls.h>

zx_status_t zx_handle_close_many(zx_handle_t* handles, size_t num_handles);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **handle_close_many**() closes a number of *handle*s, causing each
underlying object to be reclaimed by the kernel if no other handles to
it exist. -->
**handle_close()** 操作关闭*handles*数组中的所有句柄，并触发内核回收所有没有其他句柄引用的底层对象。


<!-- If a *handle* was used in a pending [object_wait_one](syscalls/object_wait_one.md) or a
[object_wait_many](syscalls/object_wait_many.md) call, the wait will be aborted. -->
如果*handles*在挂起的[object_wait_one](object_wait_one.md)或[object_wait_many](object_wait_many.md)调用中使用，则等待将被中止。

<!-- This operation closes all handles presented to it, even if one or more
of the handles is duplicate or invalid. -->
即使一个或多个句柄重复或无效，此操作也会关闭传递给它的所有句柄。

<!-- It is not an error to close the special "never a valid handle" **ZX_HANDLE_INVALID**,
similar to free(NULL) being a valid call. -->
关闭特殊的“永远无效的句柄”(**ZX_HANDLE_INVALID**)不是错误，其类似于free(NULL)，是有效的调用。

<!-- ## RIGHTS -->
## 权限

<!-- No rights are required. -->
无需任何权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **handle_close_many**() returns **ZX_OK** on success. -->
**handle_close_many()** 成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  One of the *handles* isn't a valid handle, or the same handle is
present multiple times. -->
**ZX_ERR_BAD_HANDLE**：*handles*中存在无效句柄，或同一句柄在*handles*中出现多次。

<!-- ## SEE ALSO -->
## 另见

<!-- [handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[handle_replace](handle_replace.md). -->
[handle_close](handle_close.md)，[handle_duplicate](handle_duplicate.md)，[handle_replace](handle_replace.md)。

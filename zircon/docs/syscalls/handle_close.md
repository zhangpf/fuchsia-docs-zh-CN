# zx_handle_close
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/handle_close.md)

---
<!-- ## NAME -->
## 名称

<!-- handle_close - close a handle -->
handle_close —— 关闭句柄

<!-- ## SYNOPSIS -->
## 概览

```
#include <zircon/syscalls.h>

zx_status_t zx_handle_close(zx_handle_t handle);
```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**handle_close**() closes a *handle*, causing the underlying object to be
reclaimed by the kernel if no other handles to it exist. -->
**handle_close()** 操作关闭*handle*，如果没有其他句柄指向该底层对象则触发内核回收该对象。

<!-- If the *handle* was used in a pending [object_wait_one](syscalls/object_wait_one.md) or a
[object_wait_many](syscalls/object_wait_many.md) call, the wait will be aborted. -->
如果*handle*在挂起的[object_wait_one](object_wait_one.md)或[object_wait_many](object_wait_many.md)调用中使用，则等待将被中止。

<!-- It is not an error to close the special "never a valid handle" **ZX_HANDLE_INVALID**,
similar to free(NULL) being a valid call. -->
关闭特殊的“永远无效的句柄”(**ZX_HANDLE_INVALID**)不是错误，其类似于free(NULL)，是有效的调用。

<!-- ## RIGHTS -->
## 权限

<!-- No rights are required. -->
无需任何权限。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **handle_close**() returns **ZX_OK** on success. -->
**handle_close()** 成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* isn't a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- ## SEE ALSO -->
## 另见

<!-- [handle_close_many](handle_close_many.md),
[handle_duplicate](handle_duplicate.md),
[handle_replace](handle_replace.md). -->

[handle_close_many](handle_close_many.md)，[handle_duplicate](handle_duplicate.md)，[handle_replace](handle_replace.md)。
# zx_vmar_unmap_handle_close_thread_exit
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmar_unmap_handle_close_thread_exit.md)

---
<!-- ## NAME -->
## 名称
<!-- 
vmar_unmap_handle_close_thread_exit - unmap memory, close handle, exit -->
vmar_unmap_handle_close_thread_exit —— 解除内存映射，关闭相关的句柄，并退出线程。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmar_unmap_handle_close_thread_exit(zx_handle_t vmar_handle,
                                                   zx_vaddr_t addr, size_t size,
                                                   zx_handle_t close_handle);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmar_unmap_handle_close_thread_exit**() does a sequence of three operations: -->
**vmar_unmap_handle_close_thread_exit**()依照顺序执行下列三个操作：
1. `zx_vmar_unmap(vmar_handle, addr, size);`
2. `zx_handle_close(close_handle);`
3. `zx_thread_exit();`

<!-- The expectation is that the first operation unmaps a region including the
calling thread's own stack.  (It's not required, but it's permitted.)  This
is valid for this call, though it would be invalid for *zx_vmar_unmap*() or
any other call. -->
此调用期望的结果是第一个操作取消包括调用线程自己的堆栈在内的映射区域（虽然这不是必需的，但它是允许的）。
这仅对于此调用有效，但对于*zx_vmar_unmap()* 或任何其他调用将是无效的。

<!-- If the *vmar_unmap* operation is successful, then this call never returns.
If `close_handle` is an invalid handle so that the *handle_close* operation
fails, then the thread takes a trap (as if by `__builtin_trap();`). -->
如果*vmar_unmap*操作成功，则此调用永远不再返回。 
如果`close_handle`是一个无效的句柄，以致*handle_close*操作失败，那么该线程会产生一次错误退出（正如`__builtin_trap()`一样）。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmar_unmap_handle_close_thread_exit**() does not return on success. -->
**vmar_unmap_handle_close_thread_exit()** 调用成功后不再返回

<!-- ## ERRORS -->
## 错误码

<!-- Same as [*zx_vmar_unmap*()](vmar_unmap.md). -->

和[*zx_vmar_unmap*()](vmar_unmap.md)系统调用一样。

<!-- ## NOTES -->
## 注释

<!-- The intended use for this is for a dying thread to unmap its own stack,
close its own thread handle, and exit.  The thread handle cannot be closed
beforehand because closing the last handle to a thread kills that thread.
The stack cannot be unmapped beforehand because the thread must have some
stack space on which to make its final system calls. -->
本调用的设计目的是让一个垂死的线程取消映射它自己的堆栈，关闭它自己的线程句柄，然后退出。 
线程句柄不能事先关闭，因为关闭线程的最后一个句柄会杀死该线程。 
堆栈无法事先取消映射，因为线程必须有一些堆栈空间才能执行最终的系统调用。

<!-- This call is used for detached threads, while
[*futex_wake_handle_close_thread_exit*()](futex_wake_handle_close_thread_exit.md)
is used for joinable threads. -->
此调用用于分离的(deteched)线程，而[*futex_wake_handle_close_thread_exit()*](futex_wake_handle_close_thread_exit.md)则用于joinable线程。
<!-- ## SEE ALSO -->
## 另见

[vmar_unmap](vmar_unmap.md),
[handle_close](handle_close.md),
[thread_exit](thread_exit.md),
[futex_wake_handle_close_thread_exit](futex_wake_handle_close_thread_exit.md).
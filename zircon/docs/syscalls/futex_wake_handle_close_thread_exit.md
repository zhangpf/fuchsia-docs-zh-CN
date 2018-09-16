# zx_futex_wake_handle_close_thread_exit
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/futex_wake_handle_close_thread_exit.md)

---
<!-- ## NAME -->
## 名称

<!-- futex_wake_handle_close_thread_exit - write to futex, wake futex, close handle, exit -->
futex_wake_handle_close_thread_exit —— 原子地写入futex，唤醒futex，关闭相关的句柄，并退出线程。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

_Noreturn void zx_futex_wake_handle_close_thread_exit(
    const zx_futex_t* value_ptr, uint32_t wake_count,
    int new_value, zx_handle_t close_handle);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **futex_wake_handle_close_thread_exit**() does a sequence of four operations: -->
**futex_wake_handle_close_thread_exit()** 依次执行以下四个操作：
1. `atomic_store_explicit(value_ptr, new_value, memory_order_release);`
2. `zx_futex_wake(value_ptr, wake_count);`
3. `zx_handle_close(close_handle);`
4. `zx_thread_exit();`

<!-- The expectation is that as soon as the first operation completes,
other threads may unmap or reuse the memory containing the calling
thread's own stack.  This is valid for this call, though it would be
invalid for plain *zx_futex_wake*() or any other call. -->
设计目的是在上述第一个操作完成后，其他线程可以取消映射(unmap)或重用包含调用线程的堆栈内存。因此仅对于此调用有效，而对于*zx_futex_wake()* 等其他调用无效。

<!-- If any of the operations fail, then the thread takes a trap (as if by `__builtin_trap();`). -->
如果其中任何操作失败，那么该线程会产生一次“自陷”效果（就像`__builtin_trap();`一样）。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **futex_wake_handle_close_thread_exit**() does not return. -->
**futex_wake_handle_close_thread_exit()** 不再返回

<!-- ## ERRORS -->
## 错误码

<!-- None. -->
无

<!-- ## NOTES -->
## 注释

<!-- The intended use for this is for a dying thread to alert another thread
waiting for its completion, close its own thread handle, and exit.
The thread handle cannot be closed beforehand because closing the last
handle to a thread kills that thread.  The write to `value_ptr` can't be
done before this call because any time after the write, a joining thread might
reuse or deallocate this thread's stack, which may cause issues with calling
conventions into this function. -->
该系统调用目的的是让一个将结束线程，提醒另一个线程等待它的结束，并关闭自己的线程句柄，然后退出。线程句柄不能事先被关闭，因为关闭线程的最后一个句柄会直接杀死该线程。在该调用之前无法写入`value_ptr`，因为在写入之后的任何时候，某个可结合线程可能会重用或解分配此线程的堆栈，进而导致在进入此函数的调用约定上产生问题。

<!-- This call is used for joinable threads, while
[*vmar_unmap_handle_close_thread_exit*()](vmar_unmap_handle_close_thread_exit.md)
is used for detached threads. -->
此调用仅用于可结合(joinable)线程，而对于分离(detached)线程请使用[*vmar_unmap_handle_close_thread_exit()*](vmar_unmap_handle_close_thread_exit.md)。

<!-- ## SEE ALSO -->
## 另见

<!-- [futex_wake](futex_wake.md),
[handle_close](handle_close.md),
[thread_exit](thread_exit.md),
[vmar_unmap_handle_close_thread_exit](vmar_unmap_handle_close_thread_exit.md). -->

[futex_wake](futex_wake.md)，[handle_close](handle_close.md)，[thread_exit](thread_exit.md)，[vmar_unmap_handle_close_thread_exit](vmar_unmap_handle_close_thread_exit.md)。

# zx_thread_exit
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/91786394a9cbb7b5ddd922ec761629eecd942203/docs/syscalls/thread_exit.md)

---
<!-- ## NAME -->
## 名称

<!-- thread_exit - terminate the current running thread -->
thread_exit —— 终止当前线程的执行

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

void zx_thread_exit(void);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **thread_exit**() causes the currently running thread to cease
running and exit. -->
**thread_exit()** 的功能是停止运行当前运行的线程并退出。

<!-- The signal *ZX_THREAD_TERMINATED* will be assserted on the thread
object upon exit and may be observed via *object_wait_one*()
or *object_wait_many*() on a handle to the thread. -->
信号*ZX_THREAD_TERMINATED*将在线程退出时在线程对象上被置位，并且可以通过在线程句柄上调用*object_wait_one()*或*object_wait_many()* 来获取到。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **thread_exit**() does not return. -->
**thread_exit()** 不再返回。
<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[thread_create](thread_create.md),
[thread_start](thread_start.md).
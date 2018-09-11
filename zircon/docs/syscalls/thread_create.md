# zx_thread_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/91786394a9cbb7b5ddd922ec761629eecd942203/docs/syscalls/thread_create.md)

---
<!-- ## NAME -->
## 名称

<!-- thread_create - create a thread -->
thread_create —— 创建新线程

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_thread_create(zx_handle_t process, const char* name, size_t name_size,
                             uint32_t options, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **thread_create**() creates a thread within the specified process. -->
**thread_create()** 的功能是在指定的进程中创建新线程。

<!-- Upon success a handle for the new thread is returned.  The thread
will not start executing until *thread_start()* is called. -->
执行成功后，将返回新线程的句柄，但在调用*thread_start()* 之前，该线程都不会执行。

<!-- *name* is silently truncated to a maximum of *ZX_MAX_NAME_LEN-1* characters. -->
*name*以静默的方式被截断为最多*ZX_MAX_NAME_LEN-1*个字符。

<!-- Thread handles may be waited on and will assert the signal
*ZX_THREAD_TERMINATED* when the thread stops executing (due to
*thread_exit**() being called). -->
线程句柄可以被等待，并在线程停止执行时（由于调用**thread_exit()**）发出*ZX_THREAD_TERMINATED*信号。

<!-- *process* is the controlling [process object](../objects/process.md) for the
new thread, which will become a child of that process. -->
*process*是控制新线程的[进程对象](../objects/process.md)，并且新线程将作为该进程的一个子对象。

<!-- For thread lifecycle details see [thread object](../objects/thread.md). -->
有关线程生命周期的详细信息，请参见[线程对象](../objects/thread.md)。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- On success, **thread_create**() returns **ZX_OK** and a handle (via *out*)
to the new thread.  In the event of failure, a negative error value is
returned. -->
**thread_create()** 调用成功则返回*ZX_OK*和（通过*out*）返回新线程的句柄。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *process* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*process*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *process* is not a process handle. -->
**ZX_ERR_WRONG_TYPE**：*process*不是进程类型的句柄。

<!-- **ZX_ERR_INVALID_ARGS**  *name* or *out* was an invalid pointer, or *options* was
non-zero. -->
**ZX_ERR_INVALID_ARGS**：*name*或*out*是无效指针，或*options*是非零值。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[thread_exit](thread_exit.md),
[thread_start](thread_start.md).
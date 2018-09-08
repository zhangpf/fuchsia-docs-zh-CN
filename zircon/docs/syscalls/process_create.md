# zx_process_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/c90f6a56c60f0484be9f610096fb8d58edfef424/docs/syscalls/process_create.md)

---
<!-- ## NAME -->
## 名称

<!-- process_create - create a new process -->
process_create —— 创建新进程

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_process_create(zx_handle_t job, const char* name, size_t name_size,
                              uint32_t options, zx_handle_t* proc_handle,
                              zx_handle_t* vmar_handle);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **process_create**() creates a new process. -->
**process_create()** 的功能是创建新的进程

<!-- Upon success, handles for the new process and the root of its address space
are returned.  The thread will not start executing until *process_start()* is
called. -->
执行成功后，将返回新进程的句柄以及其根地址空间。 
但在调用*process_start()* 之前，线程不会开始执行。

<!-- *name* is silently truncated to a maximum of *ZX_MAX_NAME_LEN-1* characters. -->
*name*以静默的方式被截断为最多*ZX_MAX_NAME_LEN-1*个字符。

<!-- When the last handle to a process is closed, the process is destroyed. -->
当指向进程的最后一个句柄被关闭时，该进程将被销毁。

<!-- Process handles may be waited on and will assert the signal
*ZX_PROCESS_TERMINATED* when the process exits. -->
进程句柄可以被用于等待，并在进程退出时发出*ZX_PROCESS_TERMINATED*信号。

<!-- *job* is the controlling [job object](../objects/job.md) for the new
process, which will become a child of that job. -->
*job*是控制新进程的[作业对象](../objects/job.md)，新进程将成为该作业的子进程。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- On success, **process_create**() returns **ZX_OK**, a handle to the new process
(via *proc_handle*), and a handle to the root of its address space (via
*vmar_handle*).  In the event of failure, a negative error value is returned. -->
**process_create()** 调用成功则返回*ZX_OK*和（通过*proc_handle*）返回新进程的句柄，以及（通过*vmar_handle*）返回其根地址空间。如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *job* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*job*是无效句柄。


<!-- **ZX_ERR_WRONG_TYPE**  *job* is not a job handle. -->
**ZX_ERR_WRONG_TYPE**：*job*不是作业类型的句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *job* does not have the **ZX_RIGHT_WRITE** right
(only when not **ZX_HANDLE_INVALID**). -->
**ZX_ERR_ACCESS_DENIED**：*job*没有**ZX_RIGHT_WRITE**权限（仅当*job*不是**ZX_HANDLE_INVALID**的条件下）。

<!-- **ZX_ERR_INVALID_ARGS**  *name*, *proc_handle*, or *vmar_handle*  was an invalid pointer,
or *options* was non-zero. -->
**ZX_ERR_INVALID_ARGS**：*name*，*proc_handle*或*vmar_handle*是无效指针或*options*是非零值。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_BAD_STATE**  The job object is in the dead state. -->
**ZX_ERR_BAD_STATE**：*job*作业对象处于已死亡状态。

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[process_start](process_start.md),
[task_kill](task_kill.md),
[thread_create](thread_create.md),
[thread_exit](thread_exit.md),
[thread_start](thread_start.md),
[job_create](job_create.md).
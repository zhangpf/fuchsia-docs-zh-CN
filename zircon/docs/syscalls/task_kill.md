# zx_task_kill
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/af07ad38812f7566b6c859238ece1bb4c70b969e/docs/syscalls/task_kill.md)

---
<!-- ## NAME -->
## 名称

<!-- task_kill - Kill the provided task (job, process, or thread). -->
task_kill —— 终止指定的任务（作业，进程或线程）。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_task_kill(zx_handle_t handle);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- This asynchronously kills the given process, thread or job and its children
recursively, until the entire task tree rooted at *handle* is dead. -->
该系统调用会以递归方式异步杀死给定的进程，线程或作业及其所有子任务，直到以*handle*为根节点的整个任务树终止。

<!-- It is possible to wait for the task to be dead via the **ZX_TASK_TERMINATED**
signal. When the procedure completes, as observed by the signal, the task and
all its children are considered to be in the dead state and most operations
will no longer succeed. -->
可以通过**ZX_TASK_TERMINATED**信号等待任务终止。 
正如信号所观察到的调用过程完成时，指定的任务及其所有子任务可认为已处于终止状态，并且大多数在它们上面的操作都不会成功。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- On success, **zx_task_kill**() returns **ZX_OK**. If a process or thread uses
this syscall to kill itself, this syscall does not return. -->
**zx_task_kill()** 调用成功则返回*ZX_OK*。
如果进程或线程使用此系统调用来终止自己的执行，则此调用将不会返回。

<!-- ## NOTES -->
## 注释

<!-- When using this syscall on a process, the return code for the process
is -1 as reported by **object_get_info**() via the ZX_INFO_PROCESS topic. -->
在进程上使用此系统调用时，进程的退出码为-1。
该退出码可通过传递ZX_INFO_PROCESS主题的**object_get_info()** 调用来获取。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。


<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a task handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是任务类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have the **ZX_RIGHT_DESTROY**
right. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_DESTROY**权限。

<!-- ## SEE ALSO -->
## 另见

[job_create](job_create.md),
[process_create](process_create.md).
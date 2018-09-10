# zx_task_suspend
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/af07ad38812f7566b6c859238ece1bb4c70b969e/docs/syscalls/task_suspend.md)

---
<!-- ## NAME -->
## 名称

<!-- task_suspend - suspend the given task. Currently only thread handles
may be suspended. -->
task_suspend —— 挂起指定的任务。
目前只有线程句柄可以被挂起。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

void zx_task_suspend(zx_handle_t task, zx_handle_t* suspend_token);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **task_suspend**() causes the requested task to suspend
execution. Task suspension is not synchronous and the task might not
be suspended before the call returns. The task will be suspended soon
after **task_suspend**() is invoked, unless it is currently blocked in
the kernel, in which case it will suspend after being unblocked. -->
**task_suspend()** 可使被请求的任务挂起并暂停执行。 
任务的挂起不是同步执行的，并且在该调用返回之前可能都不会挂起任务。 
调用**task_suspend()** 后，任务将很快被挂起，除非它当前在内核中处于被阻塞的状态，在这种情况下，它将在被解除阻塞后立即挂起。

<!-- Invoking **task_kill**() on a task that is suspended will successfully kill
the task. -->
在挂起的任务上调用**task_kill()** 可以成功终止任务。

<!-- ## RESUMING -->
## 恢复

<!-- The allow the task to resume, close the suspend token handle. The task will
remain suspended as long as there are any open suspend tokens. Like suspending,
resuming is asynchronous so the thread may not be in a running state when the
[handle_close](handle_close.md) call returns, even if no other suspend tokens
are open. -->
挂起令牌(suspend token)句柄被关闭时，可以使得任务被恢复，但只要有任何打开的挂起令牌存在，任务都将保持挂起状态。 
与任务挂起一样，恢复也是异步的，因此当[handle_close](handle_close.md)调用返回时，即使没有其他挂起令牌处于打开状态，线程也可能尚未处于运行状态。


<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **task_suspend**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->
**task_suspend()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE** *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** *handle* is not a thread handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是线程类型句柄。

<!-- **ZX_ERR_INVALID_ARGS**  *suspend_token*  was an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*suspend_token*是无效指针。

<!-- **ZX_ERR_BAD_STATE**  The task is not in a state where suspending is possible. -->
**ZX_ERR_BAD_STATE**：任务未处于可以被挂起的状态。

<!-- ## LIMITATIONS -->
## 限制

<!-- Currently only thread handles are supported. -->
目前仅支持线程句柄的参数。
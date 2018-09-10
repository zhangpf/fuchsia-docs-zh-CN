# zx_task_resume
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/af07ad38812f7566b6c859238ece1bb4c70b969e/docs/syscalls/task_resume.md)

---
<!-- This function is deprecated. When you suspend a thread with
[task_suspend_token](task_suspend_token.md) closing the suspend token will
automatically resume the thread.
And when you want to resume a thread from an exception,
use [task_resume_from_exception](task_resume_from_exception.md]. -->
不推荐使用此函数。 
当使用[task_suspend_token](task_suspend_token.md)挂起线程时，关闭挂起令牌句柄将自动恢复该线程的执行。 
如果想要从异常中恢复线程，请使用[task_resume_from_exception](task_resume_from_exception.md)。

<!-- ## NAME -->
## 名称

<!-- task_resume - resume the given task -->
task_resume —— 恢复指定的任务

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_task_resume(zx_handle_t task, uint32_t options);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **task_resume**() causes the requested task to resume execution, either from
an exception or from having been suspended. -->
**task_resume()** 的功能是使指定的任务从异常或被挂起状态中恢复执行。

<!-- ### RESUMING FROM SUSPEND -->
### 从挂起中恢复
<!-- If **ZX_RESUME_EXCEPTION** is not set in *options*, the operation is a
resume-from-suspend. -->
如果*options*未设置**ZX_RESUME_EXCEPTION**选项，则该操作从中断处恢复执行。

<!-- It is an error for *options* to have any options selected in this mode. -->
*options*在此模式下具有有选项都会产生错误。

<!-- **task_resume**() is invoked on a task that is not suspended but is living,
it will still return success.  If **task_resume**() is invoked multiple times
on the same suspended task, the calls are coalesced into a single resume. -->
**task_resume()** 在未挂起但仍存活（未被终止）的任务上被调用时，它仍将返回成功。 
如果在同一个挂起的任务上多次调用**task_resume()**，则会将这些调用合并为一个恢复操作。

<!-- The task will be resumed soon after **task_resume**() is invoked.  If
**task_suspend**() had been issued already, but the task had not suspended
yet, **task_resume**() cancels the pending suspend.  If a task has successfully
suspended already, **task_resume**() will resume it immediately.  A subsequent
**task_suspend**() will cause a new suspend to occur. -->
**task_resume()** 被调用后，任务将很快被恢复。 
如果已经发出了**task_suspend()**，但该任务尚未挂起，则**task_resume()** 会取消等待中的挂起。 
如果某个任务已经成功挂起，**task_resume()** 将立即恢复它的执行，随后的**task_suspend()** 将触发该任务上新的挂起。

<!-- ### RESUMING FROM EXCEPTION -->
### 从异常中恢复
<!-- Resuming from exceptions uses the same mechanism as resuming from
suspensions: **task_resume**(). An option is passed specifying that
the task is being resumed from an exception: **ZX_RESUME_EXCEPTION**. -->
从异常中恢复与从挂起中恢复使用相同的机制，即**task_resume()**。 
传递**ZX_RESUME_EXCEPTION**选项到*options*参数，表示该调用是从异常中恢复任务。

<!-- Note that a thread can be both suspended and in an exception, each
requiring separate calls to **task_resume**() with appropriate options. -->
请注意，线程可能同时处于挂起和异常状态中，此时每个状态都需要使用适当的*options*选项单独调用**task_resume()** 来恢复。

<!-- There are two ways to resume from an exception, depending on whether
one wants the thread to resume where it left off, which in the case
of an architectural exception generally means retrying the offending
instruction, or give the next handler in the search order a chance
to handle the exception.
See [task_bind_exception_port](task_bind_exception_port.md)
for a description of exception processing. -->
有两种方法可以从异常中恢复，具体取决于是否希望线程从中断处恢复，这在体系结构相关的异常的情况下通常意味着重试发生异常的指令。
否则交予搜索顺序中的下一个处理程序来处理异常。 
有关异常处理的描述，请参见[task_bind_exception_port](task_bind_exception_port.md)。

<!-- To resume a thread where it left off: -->
从中断处恢复线程：
```
zx_status_t status = zx_task_resume(thread, ZX_RESUME_EXCEPTION);
```

<!-- To pass the exception on to the next handler in the search order,
pass **ZX_RESUME_TRY_NEXT** in addition to
**ZX_RESUME_EXCEPTION**: -->
为了将异常传递给搜索顺序中的下一个处理程序，除了**ZX_RESUME_EXCEPTION**，请同时·将**ZX_RESUME_TRY_NEXT**传递给*option*参数：

```
zx_status_t status = zx_task_resume(thread,
                                    ZX_RESUME_EXCEPTION |
                                    ZX_RESUME_TRY_NEXT);
```

<!-- Note that even though exceptions are sent to handlers in a specific
order, there is no way for the caller of **zx_task_resume**()
to verify it is that handler. Anyone with appropriate rights
can resume a thread from an exception. It is up to exception
handlers to not trip over each other, as well as all other
software calling **zx_task_resume**() with **ZX_RESUME_EXCEPTION**.
(ZX-562 documents this issue.) -->
请注意，即使按特定顺序将异常发送给处理程序，**zx_task_resume()** 的调用者也无法验证它是否是该处理程序。 
具有适当权限的任何人都可以从异常中恢复线程。 
所以需要异常处理程序以及所有其他传递**ZX_RESUME_EXCEPTION**选项调用**zx_task_resume()** 的程序，来维持它们之间互不干扰。
（ZX-562记录了该问题的详细信息）

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **task_resume**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->
**task_resume()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE** *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** *handle* is not a thread handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是线程类型句柄。
<!-- **ZX_ERR_BAD_STATE**  The task is not in a state where resuming is possible (e.g.
it is dead or **ZX_RESUME_EXCEPTION** was passed but the thread is not in an
exception). -->
**ZX_ERR_BAD_STATE**：任务不处于可以恢复状态（例如，它已经终止或者传递了**ZX_RESUME_EXCEPTION**到*option*中但线程不在异常中）。

<!-- **ZX_ERR_INVALID_ARGS** *options* is not a valid combination. -->
**ZX_ERR_INVALID_ARGS**：*options*是无效的组合。

<!-- ## LIMITATIONS -->
## 限制

<!-- Currently only thread handles are supported. -->
目前仅支持线程句柄的参数。

<!-- ## SEE ALSO -->
## 另见

[task_suspend](task_suspend.md),
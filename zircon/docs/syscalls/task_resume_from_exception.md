# zx_task_resume_from_exception
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/af07ad38812f7566b6c859238ece1bb4c70b969e/docs/syscalls/task_resume_from_exception.md)

---
<!-- ## NAME -->
## 名称

<!-- task_resume_from_exception - resume the given task after an exception has been
reported -->
task_resume_from_exception —— 在报告异常后恢复指定的任务

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_task_resume_from_exception(zx_handle_t task, zx_handle_t port, uint32_t options);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **task_resume_from_exception**() causes the requested task to resume after an
exception has been reported to the debug exception port. The port parameter
should identify the [exception port](task_bind_exception_port.md) to which the
exception being resumed from was delivered. -->
**task_resume_from_exception()** 的功能是恢复向调试异常端口报告异常后的任务。 
*port*参数应标识要从中恢复异常的[异常端口](task_bind_exception_port.md)。

<!-- Note that if a thread has any open [suspend tokens](task_suspend_token.md), it
will remain suspended even when resumed from an exception. -->
请注意，如果线程有任何打开的[挂起令牌](task_suspend_token.md)，即使从异常中恢复，该线程也将保持挂起状态。

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

<!-- To resume a thread where it left off, pass 0 for the options: -->
为了从中断处恢复线程，请将0传递给*option*：
```
zx_status_t status = zx_task_resume_from_exception(thread, 0);
```

<!-- To pass the exception on to the next handler in the search order,
pass **ZX_RESUME_TRY_NEXT** for the options. -->
为了将异常交予搜索顺序中的下一个处理程序来处理，请将**ZX_RESUME_TRY_NEXT**传递给*option*参数。
```
zx_status_t status = zx_task_resume_from_exception(thread, ZX_RESUME_TRY_NEXT);
```

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **task_resume_from_exception**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->
**task_resume_from_exception()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_ACCESS_DENIED** *port* is not the port from which the exception
report was sent. -->

<!-- **ZX_ERR_BAD_HANDLE** Either *task* or *port* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*task*或*port*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** Either *task* is not a thread handle,
or *port* is not a port handle. -->
**ZX_ERR_WRONG_TYPE**：*task*不是线程类型句柄，或者*port*不是端口类型句柄。

<!-- **ZX_ERR_BAD_STATE**  The task is not in a state where resuming is possible,
for example, it is dead or there is not an exception to resume from. -->
**ZX_ERR_BAD_STATE**：任务未处于可以恢复的状态，例如，它已经终止或者没有处于可以从中恢复的异常状态。

<!-- **ZX_ERR_INVALID_ARGS** *options* is not valid. -->
**ZX_ERR_INVALID_ARGS**：*options*不合法。

<!-- ## LIMITATIONS -->
## 限制

<!-- Currently only thread handles are supported. -->
目前仅支持线程类型句柄。

<!-- ## SEE ALSO -->
## 另见

[task_bind_exception_port](task_bind_exception_port.md),
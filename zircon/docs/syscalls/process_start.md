# zx_process_start
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/c90f6a56c60f0484be9f610096fb8d58edfef424/docs/syscalls/process_start.md)

---
<!-- ## NAME -->
## 名称

<!-- process_start - start execution on a process -->
process_start —— 开始执行进程

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_process_start(zx_handle_t handle, zx_handle_t thread,
                             zx_vaddr_t entry, zx_vaddr_t stack,
                             zx_handle_t arg1, uintptr_t arg2);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **process_start**() is similar to **thread_start**(), but is used for the
purpose of starting the first thread in a process. -->
**process_start()** 的功能类似于**thread_start()**，但它只用于启动进程中的第一个线程。

<!-- **process_start**() causes a thread to begin execution at the program
counter specified by *entry* and with the stack pointer set to *stack*.
The arguments *arg1* and *arg2* are arranged to be in the architecture
specific registers used for the first two arguments of a function call
before the thread is started.  All other registers are zero upon start. -->
**process_start()** 触发线程在*entry*指定的程序计数器处开始执行，并且将堆栈指针设置为*stack*。 
参数*arg1*和*arg2*被放置在体系结构相关的寄存器中，用于在线程启动之前的函数调用的前两个参数。 
其他所有寄存器在启动时均置为零。

<!-- The first argument (*arg1*) is a handle, which will be transferred from
the process of the caller to the process which is being started, and an
appropriate handle value will be placed in arg1 for the newly started
thread. If **process_start** returns an error, *arg1* is closed rather
than transferred to the process being started. -->
第一个参数(*arg1*)是一个句柄，它将从调用者的进程传递到正在启动的进程中，并且放在arg1中的是适当的句柄值，用于新启动的线程。 
如果**process_start**返回错误，则*arg1*将被关闭，而不是传递到正在启动的进程中。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **process_start**() returns ZX_OK on success.
In the event of failure, a negative error value is returned. -->
**process_start()** 调用成功则返回*ZX_OK*。如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *process* or *thread* or *arg1* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*process*，*thread*或*arg1*是无效句柄。


<!-- **ZX_ERR_WRONG_TYPE**  *process* is not a process handle or *thread* is
not a thread handle. -->
**ZX_ERR_WRONG_TYPE**：*process*不是进程类型的句柄，或*thread*不是线程类型句柄。
<!-- 
**ZX_ERR_ACCESS_DENIED**  The handle *thread* lacks *ZX_RIGHT_WRITE* or *thread*
does not belong to *process*, or the handle *process* lacks *ZX_RIGHT_WRITE* or
*arg1* lacks ZX_RIGHT_TRANSFER. -->
**ZX_ERR_ACCESS_DENIED**：下列情况之一，*thread*没有**ZX_RIGHT_WRITE**权限；*thread*指向的线程不属于*process*进程；*process*没有**ZX_RIGHT_WRITE**权限；*arg1*没有**ZX_RIGHT_TRANSFER**权限。

<!-- **ZX_ERR_BAD_STATE**  *process* is already running or has exited. -->
**ZX_ERR_BAD_STATE**：*process*处于运行中状态或已退出。

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[process_create](process_create.md),
[thread_create](thread_create.md),
[thread_exit](thread_exit.md),
[thread_start](thread_start.md).
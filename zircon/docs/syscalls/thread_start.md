# zx_thread_start
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/91786394a9cbb7b5ddd922ec761629eecd942203/docs/syscalls/thread_start.md)

---
<!-- ## NAME -->
## 名称

<!-- thread_start - start execution on a thread -->
thread_start —— 新线程开始执行

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_thread_start(zx_handle_t handle, zx_vaddr_t entry, zx_vaddr_t stack,
                            uintptr_t arg1, uintptr_t arg2);
```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**thread_start**() causes a thread to begin execution at the program
counter specified by *entry* and with the stack pointer set to *stack*.
The arguments *arg1* and *arg2* are arranged to be in the architecture
specific registers used for the first two arguments of a function call
before the thread is started.  All other registers are zero upon start. -->
**thread_start()** 触发线程在*entry*指定的程序计数器处开始执行，并且将堆栈指针设置为*stack*。 
参数*arg1*和*arg2*被放置在体系结构相关的寄存器中，用于在线程启动之前的函数调用的前两个参数。 
其他所有寄存器在启动时均置为零。

<!-- When the last handle to a thread is closed, the thread is destroyed. -->
当线程的最后一个句柄关闭时，线程将被销毁。

<!-- Thread handles may be waited on and will assert the signal
*ZX_THREAD_TERMINATED* when the thread stops executing (due to
*thread_exit**() being called. -->
线程句柄可以被等待，并在线程停止执行时（由于**thread_exit()** 被调用）发出*ZX_THREAD_TERMINATED*信号。

<!-- 
*entry* shall point to a function that must call **thread_exit**() or
**futex_wake_handle_close_thread_exit**() or
**vmar_unmap_handle_close_thread_exit**() before reaching the last
instruction. Below is an example: -->
*entry*所指向的函数，必须在到达最后一条指令之前调用**thread_exit()**，**futex_wake_handle_close_thread_exit()** 或**vmar_unmap_handle_close_thread_exit()**。 
例如下面的例子：

<!-- ```
void thread_entry(uintptr_t arg1, uintptr_t arg2) __attribute__((noreturn)) {
	// do work here.

	zx_thread_exit();
}
``` -->
```
void thread_entry(uintptr_t arg1, uintptr_t arg2) __attribute__((noreturn)) {
	// 在这里进行一些操作
	// ……

	zx_thread_exit();
}
```

<!-- Failing to call one of the exit functions before reaching the end of
the function will cause an architecture / toolchain specific exception. -->
在函数结束之前（最后一条代码）未能调用上述其中一个退出函数将导致抛出体系结构/工具链特定的异常。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **thread_start**() returns ZX_OK on success.
In the event of failure, a negative error value is returned. -->
**thread_start()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *thread* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*thread*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *thread* is not a thread handle. -->
**ZX_ERR_WRONG_TYPE**：*thread*不是线程类型的句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  The handle *thread* lacks *ZX_RIGHT_WRITE*. -->
**ZX_ERR_ACCESS_DENIED**：*thread*缺少*ZX_RIGHT_WRITE*权限。

<!-- **ZX_ERR_BAD_STATE**  *thread* is not ready to run or the process *thread*
is part of is no longer alive. -->
**ZX_ERR_BAD_STATE**：*thread*未处于准备运行状态或*thread*不再处于存活状态。

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[thread_create](thread_create.md),
[thread_exit](thread_exit.md),
[futex_wake_handle_close_thread_exit](futex_wake_handle_close_thread_exit.md),
[vmar_unmap_handle_close_thread_exit](vmar_unmap_handle_close_thread_exit.md).
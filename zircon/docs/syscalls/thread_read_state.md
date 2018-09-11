# zx_thread_read_state
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/91786394a9cbb7b5ddd922ec761629eecd942203/docs/syscalls/thread_read_state.md)

---
<!-- ## NAME -->
## 名称

<!-- thread_read_state - Read one aspect of thread state. -->
thread_read_state —— 读取线程状态的一个方面的值

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/debug.h>

zx_status_t zx_thread_read_state(
    zx_handle_t handle,
    uint32_t kind,
    void* buffer,
    size_t len);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- 
**thread_read_state**() reads one aspect of state of the thread. The thread
state may only be read when the thread is halted for an exception or the thread
is suspended. -->
**thread_read_state()** 读取线程状态的一个方面的值。 
但只有当线程因异常而暂停或被挂起时，才能读取线程的状态。

<!-- The thread state is highly processor specific. See the structures in
zircon/syscalls/debug.h for the contents of the structures on each platform. -->
线程的状态是高度特定于处理器的。
关于每个处理器平台上结构的内容，请参阅`zircon/syscalls/debug.h`中的结构。

<!-- ## STATES -->
## 状态值

### ZX_THREAD_STATE_GENERAL_REGS

<!-- The buffer must point to a **zx_thread_state_general_regs_t** structure that
contains the general registers for the current architecture. -->
*buffer*指向包含当前体系结构通用寄存器的**zx_thread_state_general_regs_t**结构。

### ZX_THREAD_STATE_FP_REGS

<!-- The buffer must point to a **zx_thread_state_fp_regs_t** structure. On 64-bit
ARM platforms, float point state is in the vector registers and this structure
is empty. -->
*buffer*指向**zx_thread_state_fp_regs_t**结构。 
在64位ARM平台上，浮点数状态位于向量寄存器中，因此该结构为空。

### ZX_THREAD_STATE_VECTOR_REGS

<!-- The buffer must point to a **zx_thread_state_vector_regs_t** structure. -->
*buffer*指向**zx_thread_state_vector_regs_t**结构。

### ZX_THREAD_STATE_SINGLE_STEP

<!-- The buffer must point to a **zx_thread_state_single_step_t** value which
may contain either 0 (normal running), or 1 (single stepping enabled). -->
*buffer*指向**zx_thread_state_single_step_t**类型的值，该值可包含0（正常运行）或1（单步执行）。

### ZX_THREAD_X86_REGISTER_FS
<!-- 
The buffer must point to a **zx_thread_x86_register_fs_t** structure which contains
a uint64. This is only relevant on x86 platforms. -->
*buffer*指向包含uint64的**zx_thread_x86_register_fs_t**结构。 
该状态仅适用于x86平台。

### ZX_THREAD_X86_REGISTER_GS

<!-- The buffer must point to a **zx_thread_x86_register_gs_t** structure which contains
a uint64. This is only relevant on x86 platforms. -->
*buffer*指向包含uint64的**zx_thread_x86_register_gs_t**结构。 
该状态仅适用于x86平台。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **thread_read_state**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->
**thread_read_state()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not that of a thread. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是线程类型的句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* lacks *ZX_RIGHT_READ*. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少*ZX_RIGHT_READ*权限。

<!-- **ZX_ERR_INVALID_ARGS**  *kind* is not valid or *buffer* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*kind*的值无效，或*buffer*是无效指针。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_BUFFER_TOO_SMALL**  The buffer length *len* is too small to hold
the data required by *kind*. -->
**ZX_ERR_BUFFER_TOO_SMALL**：*buffer*的长度*len*太小，无法放下*kind*所需的数据。

<!-- **ZX_ERR_BAD_STATE**  The thread is not stopped at a point where state
is available. The thread state may only be read when the thread is stopped due
to an exception. -->
**ZX_ERR_BAD_STATE**：线程未在状态可用的位置停止。 
只有当线程因异常而暂停时才能读取线程的状态。

<!-- **ZX_ERR_NOT_SUPPORTED**  *kind* is not supported.
This can happen, for example, when trying to read a register set that
is not supported by the hardware the program is currently running on. -->
**ZX_ERR_NOT_SUPPORTED**：*kind*的值不受支持。 
例如，当尝试读取不受当前正在运行的硬件所支持的寄存器集时，可能会发生这种错误。

<!-- ## SEE ALSO -->
## 另见

[thread_write_state](thread_write_state.md).
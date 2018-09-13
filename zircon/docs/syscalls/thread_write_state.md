# zx_thread_write_state
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/91786394a9cbb7b5ddd922ec761629eecd942203/docs/syscalls/thread_write_state.md)

---
<!-- ## NAME -->
## 名称

<!-- thread_write_state - Write one aspect of thread state. -->
thread_write_state —— 写入线程状态的一个方面的值


<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/debug.h>

zx_status_t zx_thread_write_state(
    zx_handle_t handle,
    uint32_t kind,
    const void* buffer,
    size_t len);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **thread_write_state**() writes one aspect of state of the thread. The thread
state may only be written when the thread is halted for an exception or the
thread is suspended. -->
**thread_write_state()** 写入线程状态的一个方面的值。 
但只有当线程因异常而暂停或被挂起时，才能写入线程的状态。

<!-- The thread state is highly processor specific. See the structures in
zircon/syscalls/debug.h for the contents of the structures on each platform. -->
线程的状态是高度特定于处理器的。
关于每个处理器平台上结构的内容，请参阅`zircon/syscalls/debug.h`中的结构。

<!-- ## STATES -->
## 状态值

<!-- See [thread_read_state](thread_read_state.md) for the list of available states
and their corresponding values. -->
有关可用的线程状态及其对应值的列表，请参见[thread_read_state](thread_read_state.md)。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **thread_write_state**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->
**thread_write_state()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not that of a thread. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是线程类型的句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* lacks *ZX_RIGHT_WRITE*. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少*ZX_RIGHT_WRITE*权限。

<!-- **ZX_ERR_INVALID_ARGS**  *kind* is not valid, *buffer* is an invalid pointer,
or *len* doesn't match the size of the structure expected for *kind*. -->
**ZX_ERR_INVALID_ARGS**：*kind*的值无效，或*buffer*是无效指针；或者*len*与*kind*结构所期望的大小不匹配。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

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

[thread_read_state](thread_read_state.md).
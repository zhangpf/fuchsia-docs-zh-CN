# zx_process_read_memory
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/c90f6a56c60f0484be9f610096fb8d58edfef424/docs/syscalls/process_read_memory.md)

---
<!-- ## NAME -->
## 名称

<!-- process_read_memory - Read from the given process's address space. -->
process_read_memory —— 从给定的进程地址空间中读取数据。

<!-- ## SYNOPSIS -->
## 概要
```
#include <zircon/syscalls.h>

zx_status_t zx_process_read_memory(zx_handle_t process, zx_vaddr_t vaddr,
                                   void* buffer, size_t length, size_t* actual);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_process_read_memory**() attempts to read memory of the specified process. -->
**zx_process_read_memory()** 的功能是尝试读取指定进程的内存数据。

<!-- This function will eventually be replaced with something vmo-centric. -->
该函数最终会被一些以`vmo`为中心的函数功能所取代。

<!-- *vaddr* the address of the block of memory to read. -->
*vaddr*：需要读取的内存块地址。
<!-- *buffer* pointer to a user buffer to read bytes into. -->
*buffer*：指向用户要读取的字节的缓冲区的指针。

<!-- *length* number of bytes to attempt to read. *buffer* buffer must be large
enough for at least this many bytes.
*length* must be greater than zero and less than or equal to 64MB. -->
*length*是尝试读取的字节数。
*buffer*必须至少有*length*字节的空间。 
*length*必须大于零且不超过64MB。

<!-- *actual_size* the actual number of bytes read is stored here.
Less bytes than requested may be returned if *vaddr*+*length*
extends beyond the memory mapped in the process. -->
读取的实际字节数存储在*actual_size*中。
如果*vaddr*+*length*超出进程中映射的内存，则可能会返回比请求的字节数少的字节。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_process_read_memory**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned, and the number of
bytes written to *buffer* is undefined. -->
**zx_process_read_memory()** 调用成功则返回*ZX_OK*。如果调用失败，则返回负的错误码，并且写入到*buffer*的字节数是不确定的。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have the **ZX_RIGHT_READ** right
or
**ZX_WRITE_RIGHT** is needed for historical reasons. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_READ**权限或因历史原因需要的**ZX_WRITE_RIGHT**。

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_BAD_STATE**  the process's memory is not accessible (e.g.,
the process is being terminated),
or the requested memory is not cacheable. -->
**ZX_ERR_BAD_STATE**：进程的内存不可访问（例如，进程正在终止过程中），或者请求的内存区域不可被缓存。

<!-- **ZX_ERR_INVALID_ARGS** *buffer* is an invalid pointer or NULL,
or *length* is zero or greater than 64MB. -->
**ZX_ERR_INVALID_ARGS**：*buffer*是无效指针或为NULL，或*length*为零或大于64MB。

<!-- **ZX_ERR_NO_MEMORY** the process does not have any memory at the
requested address. -->
**ZX_ERR_NO_MEMORY**：进程在请求的地址参数上没有映射的内存。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a process handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是进程类型的句柄。

<!-- ## SEE ALSO -->
## 另见

[process_write_memory](process_write_memory.md).
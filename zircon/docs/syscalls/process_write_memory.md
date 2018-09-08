# zx_process_write_memory
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/c90f6a56c60f0484be9f610096fb8d58edfef424/docs/syscalls/process_write_memory.md)

---
<!-- ## NAME -->
## 名称

<!-- process_write_memory - Write into the given process's address space. -->
process_write_memory —— 写入数据到给定进程的地址空间中。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_process_write_memory(zx_handle_t handle, zx_vaddr_t vaddr,
                                    const void* buffer, size_t length, size_t* actual);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_process_write_memory**() attempts to write memory of the specified process. -->
**zx_process_write_memory()** 的功能是尝试写入数据到指定进程的内存中。

<!-- This function will eventually be replaced with something vmo-centric. -->
该函数最终会被一些以`vmo`为中心的函数功能所取代。

<!-- *vaddr* the address of the block of memory to write. -->
*vaddr*：将写入的内存块地址。

<!-- *buffer* pointer to a user buffer containing the bytes to write. -->
*buffer*：指向用户要写入的字节的缓冲区的指针。

<!-- *length* number of bytes to attempt to write. *buffer* buffer must be large
enough for at least this many bytes.
*length* must be greater than zero and less than or equal to 64MB. -->
*length*是尝试写入的字节数。
*buffer*必须至少有*length*字节的空间。 
*length*必须大于零且不超过64MB。

<!-- *actual_size* the actual number of bytes written is stored here.
Less bytes than requested may be returned if *vaddr*+*length*
extends beyond the memory mapped in the process. -->
写入的实际字节数存储在*actual_size*中。
如果*vaddr*+*length*超出进程中映射的内存，则可能会返回比请求的字节数少的字节。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_process_write_memory**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned, and the number of
bytes written to *buffer* is undefined. -->
**zx_process_write_memory()** 调用成功则返回*ZX_OK*。如果调用失败，则返回负的错误码，并且写入*buffer*到内存的字节数是不确定的。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have the **ZX_RIGHT_WRITE** right. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_WRITE**。

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

[process_read_memory](process_read_memory.md).
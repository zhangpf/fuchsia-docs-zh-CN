# zx_vcpu_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/18df557635b5b32816f0236ce8ee64d38bf42188/docs/syscalls/vcpu_create.md)

---
<!-- ## NAME -->
## 名称

<!-- vcpu_create - create a VCPU -->
vcpu_create —— 创建VCPU

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/hypervisor.h>

zx_status_t zx_vcpu_create(zx_handle_t guest, uint32_t options,
                           zx_vaddr_t entry, zx_handle_t* out);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vcpu_create**() creates a VCPU within a guest, which allows for execution
within the virtual machine. One or more VCPUs may be created per guest, where
the number of VCPUs does not need to match the number of physical CPUs on the
machine. -->
**vcpu_create()** 的功能是在客户虚拟机中创建VCPU，并允许其在虚拟机中执行。 
可为每个客户虚拟机创建一个或多个VCPU，其数量无需与机器上的物理CPU的数量匹配。

<!-- *entry* is the instruction pointer used to indicate where in guest physical
memory execution of the VCPU should start. -->
*entry*是用于表示VCPU开始执行的入口在客户物理内存中位置的指令指针。

<!-- *vcpu* is bound to the thread that created it, and all syscalls that operate on
it must be called from the same thread, with the exception of
**vcpu_interrupt**(). -->
*vcpu*被绑定到创建它的线程，并且必须从该线程调用操作它的所有系统调用，但**vcpu_interrupt()** 调用除外。

<!-- N.B. VCPU is an abbreviation of virtual CPU. -->
注意：VCPU是虚拟CPU的缩写。

<!-- The following rights will be set on the handle *out* by default: -->
默认情况下，句柄*out*将被设置以下权限：

<!-- **ZX_RIGHT_DUPLICATE** — *out* may be duplicated. -->
**ZX_RIGHT_DUPLICATE** —— *out*可被复制。

<!-- **ZX_RIGHT_TRANSFER** — *out* may be transferred over a channel. -->
**ZX_RIGHT_TRANSFER** —— *out*可通过通道(channel)传输。

<!-- **ZX_RIGHT_EXECUTE** — *out* may have its execution resumed (or begun) -->
**ZX_RIGHT_EXECUTE** —— *out*可恢复（或开始）执行

<!-- **ZX_RIGHT_SIGNAL** — *out* may be interrupted -->
**ZX_RIGHT_SIGNAL** —— *out*可中断执行

<!-- **ZX_RIGHT_READ** — *out* may have its state read -->
**ZX_RIGHT_READ** —— *out*可被读取其状态

<!-- **ZX_RIGHT_WRITE** — *out* may have its state written -->
**ZX_RIGHT_WRITE** —— *out*可被写入状态

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vcpu_create**() returns ZX_OK on success. On failure, an error value is
returned. -->
**vcpu_create()** 调用成功则返回**ZX_OK**。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- 
**ZX_ERR_ACCESS_DENIED** *guest* does not have the *ZX_RIGHT_MANAGE_PROCESS*
right. -->
**ZX_ERR_ACCESS_DENIED**：*guest*缺少*ZX_RIGHT_MANAGE_PROCESS*权限。

<!-- **ZX_ERR_BAD_HANDLE** *guest* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**：*guest*是无效句柄。

<!-- **ZX_ERR_INVALID_ARGS** *args* contains an invalid argument, or *out* is an
invalid pointer, or *options* is nonzero. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*args*包含无效的参数；*out*是无效指针；*options*是非零值。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_WRONG_TYPE** *guest* is not a handle to a guest. -->
**ZX_ERR_WRONG_TYPE**：*guest*不是客户虚拟机类型的句柄。

<!-- ## SEE ALSO -->
## 另见

[guest_create](guest_create.md),
[guest_set_trap](guest_set_trap.md),
[vcpu_resume](vcpu_resume.md),
[vcpu_interrupt](vcpu_interrupt.md),
[vcpu_read_state](vcpu_read_state.md),
[vcpu_write_state](vcpu_write_state.md).
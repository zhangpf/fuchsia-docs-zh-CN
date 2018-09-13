# zx_vcpu_interrupt
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/18df557635b5b32816f0236ce8ee64d38bf42188/docs/syscalls/vcpu_interrupt.md)

---
<!-- ## NAME -->
## 名称

<!-- vcpu_interrupt - raise an interrupt on a VCPU -->
vcpu_interrupt —— 在VCPU上触发中断

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vcpu_interrupt(zx_handle_t vcpu, uint32_t vector);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vcpu_interrupt**() raises an interrupt of *vector* on *vcpu*, and may be
called from any thread. -->
**vcpu_interrupt()** 的功能是触发*vcpu*上由*vector*指定的中断；该函数可以在任何线程中调用。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vcpu_interrupt**() returns ZX_OK on success. On failure, an error value is
returned. -->
**vcpu_interrupt()** 调用成功则返回**ZX_OK**。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_ACCESS_DENIED** *vcpu* does not have the *ZX_RIGHT_SIGNAL* right. -->
**ZX_ERR_ACCESS_DENIED**：*vcpu*缺少*ZX_RIGHT_SIGNAL*权限。

<!-- **ZX_ERR_BAD_HANDLE** *vcpu* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**：*vcpu*是无效句柄。

<!-- **ZX_ERR_OUT_OF_RANGE** *vector* is outside of the range interrupts supported by
the current architecture. -->
**ZX_ERR_OUT_OF_RANGE**：*vector*超出了当前体系结构所支持中断的范围。
<!-- **ZX_ERR_WRONG_TYPE** *vcpu* is not a handle to a VCPU. -->
**ZX_ERR_WRONG_TYPE**：*vcpu*不是VCPU类型的句柄。

<!-- ## SEE ALSO -->
## 另见

[guest_create](guest_create.md),
[guest_set_trap](guest_set_trap.md),
[vcpu_create](vcpu_create.md),
[vcpu_resume](vcpu_resume.md),
[vcpu_read_state](vcpu_read_state.md),
[vcpu_write_state](vcpu_write_state.md).
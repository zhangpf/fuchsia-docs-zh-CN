# zx_vcpu_write_state
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/18df557635b5b32816f0236ce8ee64d38bf42188/docs/syscalls/vcpu_write_state.md)

---
<!-- ## NAME -->
## 名称

<!-- vcpu_write_state - write the state of a VCPU -->
vcpu_write_state —— 写入VCPU的状态

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/hypervisor.h>

zx_status_t zx_vcpu_write_state(zx_handle_t vcpu, uint32_t kind,
                                const void* buffer, size_t buffer_size);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vcpu_write_state**() writes the state of *vcpu* as specified by *kind* from
*buffer*. It is only valid to write the state of *vcpu* when execution has been
paused. -->
**vcpu_write_state()** 的功能是从*buffer*中读取状态，并写入到*kind*指定的*vcpu*中。 
但只有当*vcpu*暂停执行时，写入它的状态才是有效的。

<!-- *kind* may be *ZX_VCPU_STATE* or *ZX_VCPU_IO*. -->
*kind*的值可以是*ZX_VCPU_STATE*或*ZX_VCPU_IO*。
<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vcpu_write_state**() returns ZX_OK on success. On failure, an error value is
returned. -->
**vcpu_write_state()** 调用成功则返回**ZX_OK**。
如果调用失败，则返回负的错误码。


<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_ACCESS_DENIED** *vcpu* does not have the *ZX_RIGHT_WRITE* right. -->
**ZX_ERR_ACCESS_DENIED**：*vcpu*缺少*ZX_RIGHT_WRITE*权限。

<!-- **ZX_ERR_BAD_HANDLE** *vcpu* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**：*vcpu*是无效句柄。

<!-- **ZX_ERR_BAD_STATE** *vcpu* is in a bad state, and state can not be written. -->
**ZX_ERR_BAD_STATE**：*vcpu*处于错误状态，无法写入状态。

<!-- **ZX_ERR_INVALID_ARGS** *kind* does not name a known VCPU state, *buffer* is an
invalid pointer, or *buffer_size* does not match the expected size of *kind*. -->
**ZX_ERR_INVALID_ARGS**：*kind*的值无效；或*buffer*是无效指针；或者*buffer_size*与*kind*结构所期望的大小不匹配。


<!-- **ZX_ERR_WRONG_TYPE** *vcpu* is not a handle to a VCPU. -->
**ZX_ERR_WRONG_TYPE**：*vcpu*不是VCPU类型的句柄。

<!-- ## SEE ALSO -->
## 另见

[guest_create](guest_create.md),
[guest_set_trap](guest_set_trap.md),
[vcpu_create](vcpu_create.md),
[vcpu_resume](vcpu_resume.md),
[vcpu_interrupt](vcpu_interrupt.md),
[vcpu_read_state](vcpu_read_state.md).
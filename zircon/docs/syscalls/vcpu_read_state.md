# zx_vcpu_read_state
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/18df557635b5b32816f0236ce8ee64d38bf42188/docs/syscalls/vcpu_read_state.md)

---
<!-- ## NAME -->
## 名称

<!-- vcpu_read_state - read the state of a VCPU -->
vcpu_read_state —— 读取VCPU的状态

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/hypervisor.h>

zx_status_t zx_vcpu_read_state(zx_handle_t vcpu, uint32_t kind, void* buffer,
                               size_t len);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vcpu_read_state**() reads the state of *vcpu* as specified by *kind* into
*buffer*. It is only valid to read the state of *vcpu* when execution has been
paused. -->
**vcpu_read_state()** 的功能是读取*kind*指定的*vcpu*的状态，并写入到*buffer*中。 
但只有当*vcpu*暂停执行时，写入它的状态才是有效的。

<!-- *kind* must be *ZX_VCPU_STATE*. -->
*kind*的值必须是*ZX_VCPU_STATE*。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值
<!-- 
**vcpu_read_state**() returns ZX_OK on success. On failure, an error value is
returned. -->
**vcpu_read_state()** 调用成功则返回**ZX_OK**。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- 
**ZX_ERR_ACCESS_DENIED** *vcpu* does not have the *ZX_RIGHT_READ* right. -->
**ZX_ERR_ACCESS_DENIED**：*vcpu*缺少*ZX_RIGHT_READ*权限。

<!-- **ZX_ERR_BAD_HANDLE** *vcpu* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**：*vcpu*是无效句柄。

<!-- **ZX_ERR_BAD_STATE** *vcpu* is in a bad state, and state can not be read. -->
**ZX_ERR_BAD_STATE**：*vcpu*处于错误状态，无法读取状态。

<!-- **ZX_ERR_INVALID_ARGS** *kind* does not name a known VCPU state, *buffer* is an
invalid pointer, or *len* does not match the expected size of *kind*. -->
**ZX_ERR_INVALID_ARGS**：*kind*的值无效；或*buffer*是无效指针；或者*len*与*kind*所期望的大小不匹配。

<!-- **ZX_ERR_WRONG_TYPE** *vcpu* is not a handle to a VCPU. -->
**ZX_ERR_WRONG_TYPE**：*vcpu*不是VCPU类型的句柄。

<!-- ## SEE ALSO -->
## 另见

[guest_create](guest_create.md),
[guest_set_trap](guest_set_trap.md),
[vcpu_create](vcpu_create.md),
[vcpu_resume](vcpu_resume.md),
[vcpu_interrupt](vcpu_interrupt.md),
[vcpu_write_state](vcpu_write_state.md).
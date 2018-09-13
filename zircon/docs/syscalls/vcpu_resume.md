# zx_vcpu_resume
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/18df557635b5b32816f0236ce8ee64d38bf42188/docs/syscalls/vcpu_resume.md)

---
<!-- ## NAME -->
## 名称

<!-- vcpu_resume - resume execution of a VCPU -->
vcpu_resume —— 恢复VCPU的执行

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/port.h>

zx_status_t zx_vcpu_resume(zx_handle_t vcpu, zx_port_packet_t* packet);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vcpu_resume**() begins or resumes execution of *vcpu*, and blocks until it has
paused execution. On pause of execution, *packet* is populated with reason for
the pause. After handling the reason, execution may be resumed by calling
**vcpu_resume**() again. -->
**vcpu_resume()** 的功能是开始或继续执行*vcpu*，并阻塞线程直到该*vcpu*暂停执行。 
*vcpu*暂停执行时，*packet*会被写入暂停的原因。 
在线程处理完原因后，可通过再次调用**vcpu_resume()** 恢复执行。

<!-- N.B. Execution of a *vcpu* must be resumed on the same thread it was created on. -->
注意：必须在创建*vcpu*的同一线程上恢复*vcpu*的执行。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值
<!-- 
**vcpu_resume**() returns ZX_OK on success. On failure, an error value is
returned. -->
**vcpu_resume()** 调用成功则返回**ZX_OK**。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码
<!-- 
**ZX_ERR_ACCESS_DENIED** *vcpu* does not have the *ZX_RIGHT_EXECUTE* right. -->
**ZX_ERR_ACCESS_DENIED**：*vcpu*缺少*ZX_RIGHT_EXECUTE*权限。

<!-- **ZX_ERR_BAD_HANDLE** *vcpu* is an invalid handle. -->
**ZX_ERR_BAD_HANDLE**：*vcpu*是无效句柄。

<!-- **ZX_ERR_BAD_STATE** *vcpu* is in a bad state, and can not be executed. -->
**ZX_ERR_BAD_STATE**：*vcpu*处于错误状态，无法开始执行。
<!-- 
**ZX_ERR_CANCELED** *vcpu* execution was canceled while waiting on an event. -->
**ZX_ERR_CANCELED**：*vcpu*在等待事件时执行被取消。

<!-- **ZX_ERR_INTERNAL** There was an error executing *vcpu*. -->
**ZX_ERR_INTERNAL**：在执行*vcpu*时出错。

<!-- **ZX_ERR_INVALID_ARGS** *packet* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*packet*是无效指针。

<!-- **ZX_ERR_NOT_SUPPORTED** An unsupported operation was encountered while
executing *vcpu*. -->
**ZX_ERR_NOT_SUPPORTED**：执行*vcpu*时遇到不支持的操作。

<!-- **ZX_ERR_WRONG_TYPE** *vcpu* is not a handle to a VCPU. -->
**ZX_ERR_WRONG_TYPE**：*vcpu*不是VCPU类型的句柄。

<!-- ## SEE ALSO -->
## 另见

[guest_create](guest_create.md),
[guest_set_trap](guest_set_trap.md),
[vcpu_create](vcpu_create.md),
[vcpu_interrupt](vcpu_interrupt.md),
[vcpu_read_state](vcpu_read_state.md),
[vcpu_write_state](vcpu_write_state.md).
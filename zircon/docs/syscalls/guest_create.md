# zx_guest_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/18df557635b5b32816f0236ce8ee64d38bf42188/docs/syscalls/guest_create.md)

---
<!-- ## NAME -->
## 名称

<!-- guest_create - create a guest -->
guest_create —— 创建客户虚拟机

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_guest_create(zx_handle_t resource, uint32_t options,
                            zx_handle_t* guest_handle,
                            zx_handle_t* vmar_handle);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **guest_create**() creates a guest, which is a virtual machine that can be run
within the hypervisor, with *vmar_handle* used to represent the physical address
space of the guest. -->
**guest_create()** 的功能是创建客户虚拟机，一个可以在虚拟机管理程序中运行的虚拟机，*vmar_handle*用于表示客户虚拟机的物理地址空间。

<!-- To create a guest, a *resource* of *ZX_RSRC_KIND_HYPERVISOR* must be supplied. -->
为了创建客户虚拟机，必须提供*ZX_RSRC_KIND_HYPERVISOR*类型的*资源*。

<!-- In order to begin execution within the guest, a VMO should be mapped into
*vmar_handle* using **vmar_map**(), and a VCPU must be created using
**vcpu_create**(), and then run using **vcpu_resume**(). -->
为了开始运行客户虚拟机中，应使用**vmar_map()** 将VMO映射到*vmar_handle*句柄上，并且必须使用**vcpu_create()** 创建VCPU（虚拟CPU），然后通过**vcpu_resume()** 开始运行。

<!-- Additionally, a VMO should be mapped into *vmar_handle* to provide a guest with
physical memory. -->
此外，还需将VMO映射到*vmar_handle*从而向客户虚拟机提供物理内存。

<!-- The following rights will be set on the handle *guest_handle* by default: -->
默认情况下，*guest_handle*句柄将被设置以下权限：

<!-- **ZX_RIGHT_TRANSFER** — *guest_handle* may be transferred over a channel. -->
**ZX_RIGHT_TRANSFER** —— *guest_handle*可通过通道被传输。

<!-- **ZX_RIGHT_DUPLICATE** — *guest_handle* may be duplicated. -->
**ZX_RIGHT_DUPLICATE** —— *guest_handle*可被复制。

<!-- **ZX_RIGHT_WRITE** — A trap to be may be set using **guest_set_trap**(). -->
**ZX_RIGHT_WRITE** —— 可使用**guest_set_trap()** 设置陷入中断。

<!-- **ZX_RIGHT_MANAGE_PROCESS** — A VCPU may be created using **vcpu_create**(). -->
**ZX_RIGHT_MANAGE_PROCESS** —— 可使用**vcpu_create()** 创建VCPU。

<!-- See [vmar_create](vmar_create.md) for the set of rights applied to
*vmar_handle*. -->
有关可在*vmar_handle*上设置的权限集合，请参阅[vmar_create](vmar_create.md)。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- 
**guest_create**() returns ZX_OK on success. On failure, an error value is
returned. -->
**guest_create()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_ACCESS_DENIED** *resource* is not of *ZX_RSRC_KIND_HYPERVISOR*. -->
**ZX_ERR_ACCESS_DENIED**：*resource*不是*ZX_RSRC_KIND_HYPERVISOR*类型的资源。

<!-- **ZX_ERR_INVALID_ARGS** *guest_handle* or *vmar_handle* is an invalid pointer,
or *options* is nonzero. -->
**ZX_ERR_INVALID_ARGS**：*guest_handle*或*vmar_handle*是无效指针，或*options*非零。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_WRONG_TYPE** *resource* is not a handle to a resource. -->
**ZX_ERR_WRONG_TYPE**：*resource*不是资源类型句柄。

<!-- ## SEE ALSO -->
## 另见

[guest_set_trap](guest_set_trap.md),
[vcpu_create](vcpu_create.md),
[vcpu_resume](vcpu_resume.md),
[vcpu_interrupt](vcpu_interrupt.md),
[vcpu_read_state](vcpu_read_state.md),
[vcpu_write_state](vcpu_write_state.md),
[vmar_map](vmar_map.md),
[vmo_create](vmo_create.md).
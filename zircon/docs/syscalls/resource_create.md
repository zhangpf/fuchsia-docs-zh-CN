# zx_resource_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/af07ad38812f7566b6c859238ece1bb4c70b969e/docs/syscalls/resource_create.md)

---
<!-- ## NAME -->
## 名称

<!-- resource_create - create a resource object -->
resource_create —— 创建资源对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_resource_create(zx_handle_t parent_rsrc,
                               uint32_t options,
                               uint64_t base,
                               size_t len,
                               const char* name,
                               size_t name_size,
                               zx_handle_t* out_handle)


```

<!-- ## DESCRIPTION -->
## 描述

<!-- **resource_create**() creates an resource object for use with other DDK
syscalls. Resources are typically handed out to bus drivers and rarely need to
be interacted with directly by drivers using driver protocols. Resource objects
grant access to an addresss space range starting at *base* up to but not
including *base* + *len*. Two special values for *kind* exist:
**ZX_RSRC_KIND_ROOT** and **ZX_RSRC_KIND_HYPERVISOR**. These resources have no
range associated with them and are used as a privilege check. -->
**resource_create()** 创建一个资源对象，以便其他DDK系统调用使用。
资源通常分发给总线驱动程序，并很少需要使用驱动协议与驱动程序直接交互。 
资源对象授予所有者对从*base*开始直到但不包括*base*+*len*的地址空间范围的访问权。 
存在两种*类型*的特殊资源：**ZX_RSRC_KIND_ROOT**和**ZX_RSRC_KIND_HYPERVISOR**，它们没有与之关联的范围，并仅用作权限检查。

<!-- *parent_rsrc* must be a handle to a resource of *kind* **ZX_RSRC_KIND_ROOT**. -->
*parent_rsrc*必须是值为**ZX_RSRC_KIND_ROOT***类型*资源的句柄。

<!-- 
*options* must specify which kind of resource to create and may contain optional
flags. Valid kinds of resources are **ZX_RSRC_KIND_MMIO**, **ZX_RSRC_KIND_IRQ**,
**ZX_RSRC_KIND_IOPORT** (x86 only), **ZX_RSRC_KIND_ROOT**,
**ZX_RSRC_KIND_HYPERVISOR**, and **ZX_RSRC_KIND_VMEX**.
The latter three must not be paired with non-zero values for *base* and *len*,
as they do not use an address space range.
At this time the only optional flag is **ZX_RSRC_FLAG_EXCLUSIVE**. If
**ZX_RSRC_FLAG_EXCLUSIVE** is provided then the syscall will attempt to
exclusively reserve the requested address space region, preventing other
resources creation from overlapping with it as long as it exists. -->
*options*必须指定要创建的资源类型，并可能包含其它可选的标志。 
有效的资源类型是**ZX_RSRC_KIND_MMIO**，**ZX_RSRC_KIND_IRQ**，**ZX_RSRC_KIND_IOPORT**（仅限于x86体系结构），**ZX_RSRC_KIND_ROOT**，**ZX_RSRC_KIND_HYPERVISOR**和**ZX_RSRC_KIND_VMEX**。 
后三者不能与非零值的*base*和*len*配合使用，因为它们不使用地址空间范围，此时唯一的可选标志是**ZX_RSRC_FLAG_EXCLUSIVE**。 
如果提供了**ZX_RSRC_FLAG_EXCLUSIVE**，那么系统调用将尝试独占地保留所请求的地址空间区域，以防止其他资源在创建时与之（只要它还存在）重叠。

<!-- *name* and *name_size* are optional and truncated to **ZX_MAX_NAME_LENGTH** - 1.
This name is provided for debugging / tool use only and is not used by the
kernel. -->
*name*和*name_size*参数是可选的，并且名字被透明地截断为至多**ZX_MAX_NAME_LENGTH**-1的长度。
该名字参数仅供调试/工具使用，而内核不会使用它们。

<!-- On success, a valid resource handle is returned in *out_handle*. -->
调用成功时，*out_handle*返回有效的资源句柄。

<!-- ## RETURN VALUE -->
## 返回值

<!-- **resource_create**() returns **ZX_OK** on success. In the event of failure, a
negative error value is returned. -->
**resource_create()** 执行成功则返回**ZX_OK**。如果发生错误，将返回以下错误码之一。

<!-- ## RIGHTS -->
## 权限
<!-- 
The handle will have *ZX_RIGHT_TRANSFER* (allowing it to be sent to another
process via channel write), *ZX_RIGHT_DUPLICATE* (allowing the handle to be
duplicated), *ZX_RIGHT_INSPECT* (to allow inspection of the object with
[object_get_info](object_get_info.md) and *ZX_RIGHT_WRITE* which is checked by
**resource_create**() itself. -->
句柄将具有*ZX_RIGHT_TRANSFER*（允许通过channel将其发送到另一个进程中），*ZX_RIGHT_DUPLICATE*（允许复制句柄），*ZX_RIGHT_INSPECT*（允许使用[`object_get_info`](object_get_info.md)内省对象）和*ZX_RIGHT_WRITE*权限，并由**resource_create()** 自身进行检查。

<!-- ## ERRORS -->
## 错误码
<!-- Error found!! parent_rsrc, not src_obj-->
<!-- **ZX_ERR_BAD_HANDLE** the *src_obj* handle is invalid. -->
**ZX_ERR_BAD_HANDLE**：*parent_rsrc*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** the *src_obj* handle is not a resource handle. -->
**ZX_ERR_WRONG_TYPE**：*parent_rsrc*不是资源类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED** The *src_obj* handle is not a resource of *kind*
**ZX_RSRC_KIND_ROOT**. -->
**ZX_ERR_ACCESS_DENIED**：*parent_rsrc*句柄不是**ZX_RSRC_KIND_ROOT**类型的资源。

<!-- **ZX_ERR_INVALID_ARGS** *options* contains an invalid kind or flag combination,
*name* is an invalid pointer, or the kind specified is one of
**ZX_RSRC_KIND_ROOT** or **ZX_RSRC_KIND_HYPERVISOR** but *base* and *len* are
not 0. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*options*包含无效的种类或标志的组合；*name*是无效指针；指定的类型是**ZX_RSRC_KIND_ROOT**或**ZX_RSRC_KIND_HYPERVISOR**之一但*base*和*len*不是0。

<!-- **ZX_ERR_NO_MEMORY** Failure due to lack of memory. There is no good way for
userspace to handle this (unlikely) error. In a future build this error will no
longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md), [interrupt_create](interrupt_create.md),
[ioports_requeat](ioports_request.md),
[vmo_create_physical](vmo_create_physical.md)

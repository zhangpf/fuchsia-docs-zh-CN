# zx_vmo_create_physical
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_create_physical.md)

---
<!-- ## NAME -->
## 名称
<!-- 
vmo_create_physical- create a VM object referring to a specific contiguous range
of physical memory -->
vmo_create_physical —— 创建一个指向特定连续物理内存范围的VM对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_create_physical(zx_handle_t resource, zx_paddr_t paddr,
                                   size_t size, zx_handle_t* out);
```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**vmo_create_physical**() creates a new virtual memory object (VMO), which represents the
*size* bytes of physical memory beginning at physical address *paddr*. -->
**vmo_create_physical()** 的功能是创建一个新的虚拟内存对象(VMO)，它表示从物理地址*paddr*开始的连续*size*字节的内存区域。

<!-- One handle is returned on success, representing an object with the requested
size. -->
调用成功则返回一个句柄，表示具有所请求内存大小的对象。

<!-- The following rights will be set on the handle by default: -->
默认情况下，将在句柄上设置以下权限：

<!-- **ZX_RIGHT_DUPLICATE** - The handle may be duplicated. -->
**ZX_RIGHT_DUPLICATE** —— 该句柄可被复制。

<!-- **ZX_RIGHT_TRANSFER** - The handle may be transferred to another process. -->
**ZX_RIGHT_TRANSFER** —— 该句柄可以被传输到另一个进程。

<!-- **ZX_RIGHT_READ** - May be read from or mapped with read permissions. -->
**ZX_RIGHT_READ** —— 可读或将映射权限设置为可读。

<!-- **ZX_RIGHT_WRITE** - May be written to or mapped with write permissions. -->
**ZX_RIGHT_WRITE** —— 可写或将映射权限设置为可写。

<!-- **ZX_RIGHT_EXECUTE** - May be mapped with execute permissions. -->
**ZX_RIGHT_EXECUTE** —— 可将映射权限设置为可执行。

<!-- **ZX_RIGHT_MAP** - May be mapped. -->
**ZX_RIGHT_MAP** —— 该VMO可被映射。

<!-- **ZX_RIGHT_GET_PROPERTY** - May get its properties using
[object_get_property](object_get_property). -->
**ZX_RIGHT_GET_PROPERTY** —— 可使用[object_get_property](object_get_property)获取其属性。

<!-- **ZX_RIGHT_SET_PROPERTY** - May set its properties using
[object_set_property](object_set_property). -->
**ZX_RIGHT_SET_PROPERTY** —— 可使用[object_set_property](object_set_property)设置其属性。

<!-- The **ZX_VMO_ZERO_CHILDREN** signal is active on a newly created VMO. It becomes
inactive whenever a clone of the VMO is created and becomes active again when
all clones have been destroyed and no mappings of those clones into address
spaces exist. -->
**ZX_VMO_ZERO_CHILDREN**信号在新创建的VMO时处于活跃状态。 
每当创建VMO的克隆时它就变为非活动状态，并且当该VMO所有副本都被销毁且不存在这些副本到地址空间的映射时，它才能再次变为活动状态。


<!-- ## NOTES -->
## 注释

<!-- The VMOs created by this syscall are not usable with **vmo_read**() and
**vmo_write**(). -->
此系统调用创建的VMO不能与**vmo_read()** 或**vmo_write()** 一起使用。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmo_create_physical**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**vmo_create_physical()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZER_ERR_WRONG_TYPE** *resource* is not a handle to a Resource object. -->
**ZER_ERR_WRONG_TYPE**：*resource*不是资源类型的句柄。

<!-- **ZER_ERR_ACCESS_DENIED** *resource* does not grant access to the requested
range of memory. -->
**ZER_ERR_ACCESS_DENIED**：*resource*不具有对请求的内存区域的访问权限。

<!-- **ZX_ERR_INVALID_ARGS**  *out* is an invalid pointer or NULL, or *paddr* or
*size* are not page-aligned. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*out*是无效指针或为`NULL`；*paddr*或*size*大小不是页面对齐的。


<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[vmar_map](vmar_map.md).
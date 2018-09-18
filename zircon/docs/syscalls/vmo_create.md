# zx_vmo_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_create.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_create - create a VM object -->
vmo_create —— 创建VM对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_create(uint64_t size, uint32_t options, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**vmo_create**() creates a new virtual memory object (VMO), which represents
a container of zero to *size* bytes of memory managed by the operating
system. -->
**vmo_create()** 的功能是创建一个新的虚拟内存对象（VMO），它表示由操作系统管理的0到*size*字节大小的内存容器。

<!-- The size of the VMO will be rounded up to the next page size boundary.
Use **vmo_get_size**() to return the current size of the VMO. -->
VMO的大小将向上舍入到下一页面边界的大小。 
使用**vmo_get_size()** 可获取VMO当前的大小。

<!-- One handle is returned on success, representing an object with the requested
size. -->
调用成功时返回一个句柄，表示具有所请求大小的VMO对象。

<!-- The following rights will be set on the handle by default: -->
默认情况下，将在句柄上设置以下权限：

<!-- **ZX_RIGHT_DUPLICATE** - The handle may be duplicated. -->
**ZX_RIGHT_DUPLICATE** —— 该句柄可被复制。

<!-- **ZX_RIGHT_TRANSFER** - The handle may be transferred to another process. -->
**ZX_RIGHT_TRANSFER** —— 该句柄可以被传输到另一个进程。

<!-- **ZX_RIGHT_READ** - May be read from or mapped with read permissions. -->
**ZX_RIGHT_READ** —— 可读取VMO或将映射权限设置为可读。

<!-- **ZX_RIGHT_WRITE** - May be written to or mapped with write permissions. -->
**ZX_RIGHT_WRITE** —— 可写入VMO或将映射权限设置为可写。

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

<!-- The *options* field can be 0 or **ZX_VMO_NON_RESIZABLE** to create a VMO
that cannot change size. Clones of a non-resizable VMO can be resized. -->
*options*字段可以是0或**ZX_VMO_NON_RESIZABLE**以创建不能更改大小的VMO。 
但不可调整大小的VMO的克隆副本却可以调整大小。

<!-- The **ZX_VMO_ZERO_CHILDREN** signal is active on a newly created VMO. It becomes
inactive whenever a clone of the VMO is created and becomes active again when
all clones have been destroyed and no mappings of those clones into address
spaces exist. -->
**ZX_VMO_ZERO_CHILDREN**信号在新创建的VMO时处于活跃状态。 
每当创建VMO的克隆时它就变为非活动状态，并且当该VMO所有副本都被销毁并且不存在这些副本到地址空间的映射时它才能再次变为活动状态。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmo_create**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**vmo_create()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *out* is an invalid pointer or NULL or *options* is
any value other than 0. -->
**ZX_ERR_INVALID_ARGS**：*out*是无效指针或为`NULL`；或*options*是0以外的其他任何值。


<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[vmo_read](vmo_read.md),
[vmo_write](vmo_write.md),
[vmo_clone](vmo_clone.md),
[vmo_set_size](vmo_set_size.md),
[vmo_get_size](vmo_get_size.md),
[vmo_op_range](vmo_op_range.md),
[vmar_map](vmar_map.md).
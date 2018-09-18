# zx_vmo_clone
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_clone.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_clone - create a clone of a VM Object -->
vmo_clone —— 创建VM对象的副本

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_clone(zx_handle_t handle, uint32_t options, uint64_t offset, uint64_t size, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmo_clone**() creates a new virtual memory object (VMO) that clones a range
of an existing vmo. -->
**vmo_clone()** 的功能是创建一个新的虚拟内存对象（VMO），用于克隆现有vmo的内存区域。

<!-- One handle is returned on success, representing an object with the requested
size. -->
调用成功则返回一个句柄，表示具有所请求内存大小的对象。

<!-- *options* must contain *ZX_VMO_CLONE_COPY_ON_WRITE* and zero or more flags to control
clone creation. -->
*options*必须包含*ZX_VMO_CLONE_COPY_ON_WRITE*选项，以及零个或多个标志位以控制克隆对象的创建。

<!-- Valid flags: -->
有效的标志位包括：

<!-- - *ZX_VMO_CLONE_COPY_ON_WRITE* - Create a copy-on-write clone. The cloned vmo will
behave the same way the parent does, except that any write operation on the clone
will bring in a copy of the page at the offset the write occurred. The new page in
the cloned vmo is now a copy and may diverge from the parent. Any reads from
ranges outside of the parent vmo's size will contain zeros, and writes will
allocate new zero filled pages.  See the NOTES section below for details on
VMO syscall interactions with clones. -->
- *ZX_VMO_CLONE_COPY_ON_WRITE* —— 创建写时复制（Copy-on-write）的副本。 
  除了对副本的任何写操作将在写入位置的偏移量处引入页面的新副本之外，克隆的vmo将以与原始VMO相同的方式运行。 
  vmo副本中的新页面是原页面的副本，但已与父页面分离。 
  任何来自父VMO大小范围之外的读取操作都将返回零，并且写入操作将分配新的零填充页面。
  有关与副本交互的VMO系统调用的详细信息，请参阅下面的[注释](#注释)部分。

<!-- - *ZX_VMO_CLONE_NON_RESIZEABLE* - Create a non-resizeable clone VMO. -->
- *ZX_VMO_CLONE_NON_RESIZEABLE* —— 创建不可调整大小的VMO副本。

<!-- *offset* must be page aligned. -->
*offset*大小必须是页面对齐的。
<!-- 
*offset* + *size* may not exceed the range of a 64bit unsigned value. -->
*offset* + *size*不得超过64位无符号值的范围。

<!-- Both offset and size may start or extend beyond the original VMO's size. -->
偏移量和大小都可以超出原始VMO的大小。

<!-- The size of the VMO will be rounded up to the next page size boundary. -->
VMO将向上舍入到下一页面边界的大小。

<!-- By default the rights of the cloned handled will be the same as the
original with a few exceptions. See [vmo_create](vmo_create.md) for a
discussion of the details of each right. -->
默认情况下，副本的句柄的权限与原始句柄权限相同，但有一些例外。 
有关每项权限的详细信息，请参阅[vmo_create](vmo_create.md)。

<!-- If *options* is *ZX_VMO_CLONE_COPY_ON_WRITE* the following rights are added: -->
如果*options*为*ZX_VMO_CLONE_COPY_ON_WRITE*，则会添加以下权限到句柄上：
- **ZX_RIGHT_WRITE**

<!-- ## NOTES -->
## 注释

<!-- Cloning a VMO causes the existing (source) VMO **ZX_VMO_ZERO_CHILDREN** signal
to become inactive. Only when the last clone is destroyed and no mappings
of those clones into address spaces exist, will **ZX_VMO_ZERO_CHILDREN** become
active again. -->
克隆VMO会导致现有（原始）VMO的**ZX_VMO_ZERO_CHILDREN**信号变成非活跃状态。 
只有当最后一个副本被销毁并且没有这些副本到地址空间的映射时，**ZX_VMO_ZERO_CHILDREN**才会再次激活。

### ZX_VMO_CLONE_COPY_ON_WRITE

<!-- VMOs produced by this mode will interact with the VMO syscalls in the following
ways: -->
此模式生成的VMO将通过以下方式与VMO系统调用进行交互：

<!-- - The DECOMMIT and COMMIT modes of **vmo_op_range**() on a clone will only affect pages
  allocated to the clone, never its parent. -->
- 副本上的**vmo_op_range()** 的DECOMMIT和COMMIT模式只会影响分配给副本的页面，而不会影响其父节点。
<!-- - If a page in a clone is decommitted (e.g. with **vmo_op_range**()), the parent's page will
  become visible once again, still with copy-on-write semantics. -->
- 如果副本中的页面被解除提交（例如使用**vmo_op_range()** 操作），则父页面将再次可见，并且仍具有写时复制语义。
<!-- - If a page is committed to a clone using the **vmo_op_range**() COMMIT mode, a
  the new page will have the same contents as the parent's corresponding page
  (or zero-filled if no such page exists). -->
- 如果使用**vmo_op_range()** 的COMMIT模式将页面提交到副本上，则新页面将具有与父对应页面相同的内容（如果不存在此页面，则使用零填充）。
<!-- - If the **vmo_op_range**() LOOKUP mode is used, the parent's pages will be visible
  where the clone has not modified them. -->
- 如果使用**vmo_op_range()** 的LOOKUP模式，则父节点中副本未修改的页面将仍然可见。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmo_clone**() returns **ZX_OK** on success. In the event
of failure, a negative error value is returned. -->
**vmo_clone()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码
<!-- 
**ERR_BAD_TYPE**  Input handle is not a VMO. -->
**ZX_ERR_WRONG_TYPE**：输入句柄不是VMO类型。

<!-- **ZX_ERR_ACCESS_DENIED**  Input handle does not have sufficient rights. -->
**ZX_ERR_ACCESS_DENIED**：输入句柄没有足够的权限。

<!-- **ZX_ERR_INVALID_ARGS**  *out* is an invalid pointer or NULL
or the offset is not page aligned. -->
**ZX_ERR_INVALID_ARGS**：*out*是无效指针或为`NULL`；或偏移量不是页面对齐的。

<!-- **ZX_ERR_OUT_OF_RANGE**  *offset* + *size* is too large. -->
**ZX_ERR_OUT_OF_RANGE**：*offset* + *size*太大超出了范围。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[vmo_create](vmo_create.md),
[vmo_read](vmo_read.md),
[vmo_write](vmo_write.md),
[vmo_set_size](vmo_set_size.md),
[vmo_get_size](vmo_get_size.md),
[vmo_op_range](vmo_op_range.md),
[vmar_map](vmar_map.md).
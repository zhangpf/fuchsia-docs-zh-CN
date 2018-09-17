# zx_vmar_allocate
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmar_allocate.md)

---
<!-- ## NAME -->
## 名称

<!-- vmar_allocate - allocate a new subregion -->
vmar_allocate —— 分配新的子VMAR区域

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmar_allocate(zx_handle_t parent_vmar, zx_vm_option_t options,
                             uint64_t offset, uint64_t size,
                             zx_handle_t* child_vmar, zx_vaddr_t* child_addr)
```

<!-- ## DESCRIPTION -->
## 描述

<!-- Creates a new VMAR within the one specified by *parent_vmar*. -->
在*parent_vmar*指定的VMAR中创建新的子VMAR。

<!-- *options* is a bit vector that contains one more of the following: -->
*options*是一个位向量，包含以下选项中的一个或多个：
<!-- - **ZX_VM_COMPACT**  A hint to the kernel that allocations and mappings
  within the newly created subregion should be kept close together.   See the
  NOTES section below for discussion. -->
- **ZX_VM_COMPACT**：提示内核应该保持新创建的子区域内的分配和映射关系。有关讨论，请参阅下面的[注释](#注释)部分。
<!-- - **ZX_VM_SPECIFIC**  Use the *offset* to place the mapping, invalid if
  vmar does not have the **ZX_VM_CAN_MAP_SPECIFIC** permission.  *offset*
  is an offset relative to the base address of the parent region.  It is an error
  to specify an address range that overlaps with another VMAR or mapping. -->
- **ZX_VM_SPECIFIC**：使用*offset*指定映射，但如果VMAR没有**ZX_VM_CAN_MAP_SPECIFIC**权限，则无效。 
  *offset*是相对于父区域基地址的偏移量。 
  指定的地址范围与另一个VMAR或映射有重叠部分会导致错误。
<!-- - **ZX_VM_CAN_MAP_SPECIFIC**  The new VMAR can have subregions/mappings
  created with **ZX_VM_SPECIFIC**.  It is NOT an error if the parent does
  not have *ZX_VM_CAN_MAP_SPECIFIC* permissions. -->
- **ZX_VM_CAN_MAP_SPECIFIC**：新VMAR可以使用**ZX_VM_SPECIFIC**创建子区域/映射。
  如果父级VMAR没有*ZX_VM_CAN_MAP_SPECIFIC*权限，*不*会导致错误。
<!-- - **ZX_VM_CAN_MAP_READ**  The new VMAR can contain readable mappings.
  It is an error if the parent does not have *ZX_VM_CAN_MAP_READ* permissions. -->
- **ZX_VM_CAN_MAP_READ**：新VMAR可包含可读区域的映射。 
  但如果父级VMAR没有*ZX_VM_CAN_MAP_READ*权限，则会导致错误。
<!-- - **ZX_VM_CAN_MAP_WRITE**  The new VMAR can contain writable mappings.
  It is an error if the parent does not have *ZX_VM_CAN_MAP_WRITE* permissions. -->
- **ZX_VM_CAN_MAP_WRITE**：新VMAR可包含可写区域的映射。 
  但如果父级VMAR没有*ZX_VM_CAN_MAP_WRITE*权限，则会导致错误。
<!-- - **ZX_VM_CAN_MAP_EXECUTE**  The new VMAR can contain executable mappings.
  It is an error if the parent does not have *ZX_VM_CAN_MAP_EXECUTE* permissions. -->
- **ZX_VM_CAN_MAP_EXECUTE**：新VMAR可包含可执行区域的映射。 
  但如果父级VMAR没有*ZX_VM_CAN_MAP_EXECUTE*权限，则会导致错误。
<!-- *offset* must be 0 if *options* does not have **ZX_VM_SPECIFIC** set. -->
如果*options*没有设置**ZX_VM_SPECIFIC**选项，则*offset*必须为0。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmar_allocate**() returns **ZX_OK**, the absolute base address of the
subregion (via *child_addr*), and a handle to the new subregion (via
*child_vmar*) on success.  The base address will be page-aligned and non-zero.
In the event of failure, a negative error value is returned. -->
**vmar_allocate()** 调用成功则返回**ZX_OK**，并（通过*child_addr*）返回子区域的绝对基地址，以及（通过*child_vmar*）返回新子区域的句柄，这里的基地址将是页面对齐的且非零。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *parent_vmar* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*parent_vmar*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *parent_vmar* is not a VMAR handle. -->
**ZX_ERR_WRONG_TYPE**：*parent_vmar*不是VMAR类型的句柄。

<!-- **ZX_ERR_BAD_STATE**  *parent_vmar* refers to a destroyed VMAR. -->
**ZX_ERR_BAD_STATE**：*parent_vmar*句柄指向的是已销毁的VMAR。

<!-- **ZX_ERR_INVALID_ARGS**  *child_vmar* or *child_addr* are not valid, *offset* is
non-zero when *ZX_VM_SPECIFIC* is not given, *offset* and *size* describe
an unsatisfiable allocation due to exceeding the region bounds, *offset*
or *size* is not page-aligned, or *size* is 0. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*child_vmar*或*child_addr*无效；*offset*在未指定*ZX_VM_SPECIFIC*选项时为非零；*offset*和*size*指定了由于超出区域范围而导致的不可满足的分配；*offset*或*size*不是页面对齐的；或者*size*是0。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_ACCESS_DENIED**  Insufficient privileges to make the requested allocation. -->
**ZX_ERR_ACCESS_DENIED**：没有足够的权限来执行分配请求。

<!-- ## NOTES -->
## 注释

<!-- ### The COMPACT flag -->
### COMPACT标志位

<!-- The kernel interprets this flag as a request to reduce sprawl in allocations.
While this does not necessitate reducing the absolute entropy of the allocated
addresses, there will potentially be a very high correlation between allocations.
This is a trade-off that the developer can make to increase locality of
allocations and reduce the number of page tables necessary, if they are willing
to have certain addresses be more correlated. -->
内核将此标志解释为试图减少分配产生的蔓延的请求。
虽然这不需要降低分配地址的绝对熵，但分配之间可能因此存在非常高的相关性。 
如果开发人员愿意让某些地址更加相关，那么可以通过这种权衡来增加分配的局部性并减少必要的页表数量。

<!-- ## SEE ALSO -->
## 另见

[vmar_destroy](vmar_destroy.md),
[vmar_map](vmar_map.md),
[vmar_protect](vmar_protect.md),
[vmar_unmap](vmar_unmap.md).
# zx_vmar_map
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmar_map.md)

---
<!-- ## NAME -->
## 名称

<!-- vmar_map - add a memory mapping -->
vmar_map —— 添加内存映射

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmar_map(zx_handle_t vmar, zx_vm_option_t options,
                        uint64_t vmar_offset, zx_handle_t vmo,
                        uint64_t vmo_offset, uint64_t len,
                        zx_vaddr_t* mapped_addr)
```

<!-- ## DESCRIPTION -->
## 描述

<!-- Maps the given VMO into the given virtual memory address region.  The mapping
retains a reference to the underlying virtual memory object, which means
closing the VMO handle does not remove the mapping added by this function. -->
将给定的VMO映射到给定的虚拟内存地址区域。 映射保留对底层虚拟内存对象的引用，这意味着关闭VMO句柄不会删除此函数添加的映射。

<!-- *options* is a bit vector of the following: -->
*options*是以下选项组成的位矢量：
<!-- - **ZX_VM_SPECIFIC**  Use the *vmar_offset* to place the mapping, invalid if
  vmar does not have the **ZX_VM_CAN_MAP_SPECIFIC** permission.
  *vmar_offset* is an offset relative to the base address of the given VMAR.
  It is an error to specify a range that overlaps with another VMAR or mapping. -->
- **ZX_VM_SPECIFIC**：使用*vmar_offset*参数指定映射，但如果vmar没有**ZX_VM_CAN_MAP_SPECIFIC**权限，则操作无效。 
  *vmar_offset*是相对于给定的VMAR基地址的偏移量。 
  指定的范围与另一个VMAR或映射有重叠会导致错误发生。
<!-- - **ZX_VM_SPECIFIC_OVERWRITE**  Same as **ZX_VM_SPECIFIC**, but can
  overlap another mapping.  It is still an error to partially-overlap another VMAR.
  If the range meets these requirements, it will atomically (with respect to all
  other map/unmap/protect operations) replace existing mappings in the area. -->
- **ZX_VM_SPECIFIC_OVERWRITE**：与**ZX_VM_SPECIFIC**相同，但可以与另一个映射重叠，但与另一个VMAR部分重叠仍然会导致错误。 
  如果范围满足这些要求，则它将原子地（相对于所有其他map/unmap/protect操作）替换该区域中的现有映射。
<!-- - **ZX_VM_PERM_READ**  Map *vmo* as readable.  It is an error if *vmar*
  does not have *ZX_VM_CAN_MAP_READ* permissions, the *vmar* handle does
  not have the *ZX_RIGHT_READ* right, or the *vmo* handle does not have the
  *ZX_RIGHT_READ* right. -->
- **ZX_VM_PERM_READ**：将*vmo*映射设置为可读权限。 
  如果*vmar*没有*ZX_VM_CAN_MAP_READ*权限，或*vmar*句柄没有*ZX_RIGHT_READ*权限，或者*vmo*句柄没有*ZX_RIGHT_READ*权限，则会导致错误。
<!-- - **ZX_VM_PERM_WRITE**  Map *vmo* as writable.  It is an error if *vmar*
  does not have *ZX_VM_CAN_MAP_WRITE* permissions, the *vmar* handle does
  not have the *ZX_RIGHT_WRITE* right, or the *vmo* handle does not have the
  *ZX_RIGHT_WRITE* right. -->
- **ZX_VM_PERM_WRITE**：将*vmo*映射设置为可写权限。 
  如果*vmar*没有*ZX_VM_CAN_MAP_WRITE*权限，或*vmar*句柄没有*ZX_RIGHT_WRITE*权限，或者*vmo*句柄没有*ZX_RIGHT_WRITE*权限，则会导致错误。
<!-- - **ZX_VM_PERM_EXECUTE**  Map *vmo* as executable.  It is an error if *vmar*
  does not have *ZX_VM_CAN_MAP_EXECUTE* permissions, the *vmar* handle does
  not have the *ZX_RIGHT_EXECUTE* right, or the *vmo* handle does not have the
  *ZX_RIGHT_EXECUTE* right. -->
- **ZX_VM_PERM_EXECUTE**：将*vmo*映射设置可执行权限。 
  如果*vmar*没有*ZX_VM_CAN_MAP_EXECUTE*权限，或*vmar*句柄没有*ZX_RIGHT_EXECUTE*权限，或*vmo*句柄没有*ZX_RIGHT_EXECUTE*权限，则会导致错误。
<!-- - **ZX_VM_MAP_RANGE**:  Immediately page into the new mapping all backed
  regions of the VMO.  This cannot be specified if
  *ZX_VM_SPECIFIC_OVERWRITE* is used. -->
- **ZX_VM_MAP_RANGE**：立即使用新映射VMO的所有后备区域进行分页。 
  但如果使用*ZX_VM_SPECIFIC_OVERWRITE*，则不能指定该选项。
<!-- - **ZX_VM_REQUIRE_NON_RESIZABLE** Maps the VMO only if the VMO is non-resizable,
  that is, it was created with the **ZX_VMO_NON_RESIZABLE** option. -->
- **ZX_VM_REQUIRE_NON_RESIZABLE**：仅当VMO不可调整大小时才映射VMO，即它是使用**ZX_VMO_NON_RESIZABLE**选项创建的。
<!-- *vmar_offset* must be 0 if *options* does not have **ZX_VM_SPECIFIC** or
**ZX_VM_SPECIFIC_OVERWRITE** set.  If neither of those are set, then
the mapping will be assigned an offset at random by the kernel (with an
allocator determined by policy set on the target VMAR). -->
如果*options*没有设置**ZX_VM_SPECIFIC**或**ZX_VM_SPECIFIC_OVERWRITE**选项，则*vmar_offset*必须为0。 
如果这两者都没有设置，那么内核（通过由目标VMAR上的策略集确定的分配器）将随机为映射指定一个偏移量。

<!-- *len* must be page-aligned. -->
*len*的大小必须是页面对齐的。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmar_map**() returns **ZX_OK** and the absolute base address of the
mapping (via *mapped_addr*) on success.  The base address will be page-aligned
and non-zero.  In the event of failure, a negative error value is returned. -->
**vmar_allocate()** 调用成功则返回**ZX_OK**，并（通过*mapped_addr*）返回映射的绝对基地址，这里的基地址将是页面对齐的且非零。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *vmar* or *vmo* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*vmar*或是*vmo*无效句柄。
<!-- 
**ZX_ERR_WRONG_TYPE**  *vmar* or *vmo* is not a VMAR or VMO handle, respectively. -->
**ZX_ERR_WRONG_TYPE**：*vmar*或*vmo*不是VMAR或VMO类型的句柄。

<!-- **ZX_ERR_BAD_STATE**  *vmar* refers to a destroyed VMAR. -->
**ZX_ERR_BAD_STATE**：*vmar*句柄指向的是已销毁的VMAR。

<!-- **ZX_ERR_INVALID_ARGS** *mapped_addr* or *options* are not valid, *vmar_offset* is
non-zero when neither **ZX_VM_SPECIFIC** nor
**ZX_VM_SPECIFIC_OVERWRITE** are given,
**ZX_VM_SPECIFIC_OVERWRITE** and **ZX_VM_MAP_RANGE** are both given,
*vmar_offset* and *len* describe an unsatisfiable allocation due to exceeding the region bounds,
*vmar_offset* or *vmo_offset* or *len* are not page-aligned,
*vmo_offset* + ROUNDUP(*len*, PAGE_SIZE) overflows. -->

**ZX_ERR_INVALID_ARGS**：下列情况之一，*mapped_addr*或*options*无效；在未设置**ZX_VM_SPECIFIC**或**ZX_VM_SPECIFIC_OVERWRITE**时，*vmar_offset*为非零值；同时设置了**ZX_VM_SPECIFIC_OVERWRITE**和**ZX_VM_MAP_RANGE**选项；*vmar_offset*和*size*指定了由于超出区域范围而导致的不可满足的分配；*vmar_offset*，*vmo_offset*或*size*不是页面对齐的；或*vmo_offset* + ROUNDUP(*len*，PAGE_SIZE)溢出。

<!-- **ZX_ERR_NOT_SUPPORTED** The VMO is resizable and **ZX_VM_REQUIRE_NON_RESIZABLE** was
requested. -->
**ZX_ERR_NOT_SUPPORTED**：VMO具有可调整大小权限，但在调用中设置了**ZX_VM_REQUIRE_NON_RESIZABLE**选项。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- ## NOTES -->
## 注释

<!-- The VMO that backs a memory mapping can be resized to a smaller size. This can cause the
thread is reading or writting to the VMAR region to fault. To avoid this hazard, services
that receive VMOs from clients should use **ZX_VM_REQUIRE_NON_RESIZABLE** when mapping
the VMO. -->
提供内存映射的VMO可以调整为更小规模的映射，但这可能导致线程正在读取或写入的VMAR区域发生故障。 
为避免此危险，从客户端接收VMO的服务在映射VMO时应使用**ZX_VM_REQUIRE_NON_RESIZABLE**选项。

<!-- ## SEE ALSO -->
## 另见

[vmar_allocate](vmar_allocate.md),
[vmar_destroy](vmar_destroy.md),
[vmar_protect](vmar_protect.md),
[vmar_unmap](vmar_unmap.md).
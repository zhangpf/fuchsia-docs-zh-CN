<!-- # Virtual Memory Address Region -->
# Virtual Memory Address Region（虚拟内存地址区域）
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/objects/vm_object.md)

---
<!-- ## NAME -->
## 名称

<!-- vm_address_region - A contiguous region of a virtual memory address space -->
vm_address_region —— 虚拟内存地址空间的连续区域

<!-- ## SYNOPSIS -->
## 概要

<!-- Virtual Memory Address Regions (VMARs) represent contiguous parts of a virtual
address space. -->
虚拟内存地址区域（VMAR）表示虚拟地址空间的连续部分。

<!-- ## DESCRIPTION -->
## 描述

<!-- VMARs are used by the kernel and userspace to represent the allocation of an
address space. -->
内核和用户空间使用VMAR来表示地址空间的分配。

<!-- Every process starts with a single VMAR (the root VMAR) that spans the entire
address space (see [process_create](../syscalls/process_create.md)).  Each VMAR
can be logically divided up into any number of non-overlapping parts, each
representing a child VMARs, a virtual memory mapping, or a gap.  Child VMARs
are created using [vmar_allocate](../syscalls/vmar_allocate.md).  VM mappings
are created using [vmar_map](../syscalls/vmar_map.md). -->
每个进程启动时都带有一个包含整个地址空间的VMAR（根VMAR）（请参阅[process_create](../syscalls/process_create.md)）。
每个VMAR可以在逻辑上划分为任意数量的非重叠部分，每个部分代表虚拟内存的一个映射或间隙的子VMAR。 
使用[vmar_allocate](../syscalls/vmar_allocate.md)创建子VMAR，以及通过使用[vmar_map](../syscalls/vmar_map.md)创建虚拟内存映射。

<!-- VMARs have a hierarchical permission model for allowable mapping permissions.
For example, the root VMAR allows read, write, and executable mapping.  One
could create a child VMAR that only allows read and write mappings, in which
it would be illegal to create a child that allows executable mappings. -->
VMAR具有允许映射权限的分层权限模型。 例如，根VMAR允许读取，写入和可执行映射。 可以创建仅允许读写映射的子VMAR，其中创建允许可执行映射的子项是非法的。

<!-- When a VMAR is created using vmar_allocate, its parent VMAR retains a reference
to it.  Because of this, if all handles to the child VMAR are closed, the child
and its descendents will remain active in the address space.  In order to
disconnect the child from the address space, [vmar_destroy](../syscalls/vmar_destroy.md)
must be called on a handle to the child. -->
使用vmar_allocate创建VMAR时，其父VMAR会保留对它的引用，因此，即使关闭子VMAR的所有句柄，则子对象及其后代也将在地址空间中保持存活状态。
为了将子节点与地址空间断开连接，必须在子节点的句柄上调用[vmar_destroy](../syscalls/vmar_destroy.md)。

<!-- By default, all allocations of address space are randomized.  At VMAR
creation time, the caller can choose which randomization algorithm is used.
The default allocator attempts to spread allocations widely across the full
width of the VMAR.  The alternate allocator, selected with
*ZX_VM_COMPACT*, attempts to keep allocations close together within the
VMAR, but at a random location within the range.  It is recommended to use
the default allocator. -->
默认情况下，地址空间的所有分配都是随机的，并在VMAR创建时，调用者可以选择使用哪种随机化算法。 
默认的分配器尝试在VMAR的整个空间上分配，使用*ZX_VM_COMPACT*选项的备用分配器会试图在VMAR内保持分配尽量簇聚在一起，但也是在空间范围内的随机位置。 
建议开发者使用默认的分配器。

<!-- VMARs optionally support a fixed-offset mapping mode (called specific mapping).
This mode can be used to create guard pages or ensure the relative locations of
mappings.  Each VMAR may have the *ZX_VM_CAN_MAP_SPECIFIC* permission,
regardless of whether or not its parent VMAR had that permission. -->
VMAR可选地支持固定偏移映射模式（也称为特定映射），此模式可用于创建守卫(guard)页面或确保映射的相对位置。 
每个VMAR都可能具有*ZX_VM_CAN_MAP_SPECIFIC*权限，无论其父VMAR是否具有该权限。

<!-- ## EXAMPLE -->
## 例子
<!-- 
```c
#include <zircon/syscalls.h>

/* Map this VMO into the given VMAR, with |before| bytes of unmapped guard space
   before it and |after| bytes after it.  */
zx_status_t map_with_guard(zx_handle_t vmar, size_t before, size_t after,
                           zx_handle_t vmo, uint64_t vmo_offset,
                           size_t mapping_len, uintptr_t* mapped_addr,
                           zx_handle_t* wrapping_vmar) {

    /* wrap around check elided */
    const size_t child_vmar_size = before + after + mapping_len;
    const zx_vm_option_t child_vmar_options = ZX_VM_CAN_MAP_READ |
                                              ZX_VM_CAN_MAP_WRITE |
                                              ZX_VM_CAN_MAP_SPECIFIC;
    const zx_vm_option_t mapping_options = ZX_VM_SPECIFIC |
                                           ZX_VM_PERM_READ |
                                           ZX_VM_PERM_WRITE;

    uintptr_t child_vmar_addr;
    zx_handle_t child_vmar;
    zx_status_t status = zx_vmar_allocate(vmar, child_vmar_options, 0,
                                          child_vmar_size,
                                          &child_vmar,
                                          &child_vmar_addr);
    if (status != ZX_OK) {
        return status;
    }

    status = zx_vmar_map(child_vmar, mapping_options, before, vmo, vmo_offset,
                         mapping_len, mapped_addr);
    if (status != ZX_OK) {
        zx_vmar_destroy(child_vmar);
        zx_handle_close(child_vmar);
        return status;
    }

    *wrapping_vmar = child_vmar;
    return ZX_OK;
}
``` -->


```c
#include <zircon/syscalls.h>

/* 将此VMO映射到给定的VMAR，并使用在它之前|before|字节和它之后的|after|字节作为未映射的守卫空间。  */
zx_status_t map_with_guard(zx_handle_t vmar, size_t before, size_t after,
                           zx_handle_t vmo, uint64_t vmo_offset,
                           size_t mapping_len, uintptr_t* mapped_addr,
                           zx_handle_t* wrapping_vmar) {

    /* 这里省略了相关的检查代码 */
    const size_t child_vmar_size = before + after + mapping_len;
    const zx_vm_option_t child_vmar_options = ZX_VM_CAN_MAP_READ |
                                              ZX_VM_CAN_MAP_WRITE |
                                              ZX_VM_CAN_MAP_SPECIFIC;
    const zx_vm_option_t mapping_options = ZX_VM_SPECIFIC |
                                           ZX_VM_PERM_READ |
                                           ZX_VM_PERM_WRITE;

    uintptr_t child_vmar_addr;
    zx_handle_t child_vmar;
    zx_status_t status = zx_vmar_allocate(vmar, child_vmar_options, 0,
                                          child_vmar_size,
                                          &child_vmar,
                                          &child_vmar_addr);
    if (status != ZX_OK) {
        return status;
    }

    status = zx_vmar_map(child_vmar, mapping_options, before, vmo, vmo_offset,
                         mapping_len, mapped_addr);
    if (status != ZX_OK) {
        zx_vmar_destroy(child_vmar);
        zx_handle_close(child_vmar);
        return status;
    }

    *wrapping_vmar = child_vmar;
    return ZX_OK;
}
```

<!-- ## SEE ALSO -->
## 另见

<!-- + [vm_object](vm_object.md) - Virtual Memory Objects -->
+ [vm_object](vm_object.md) —— 虚拟内存对象

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [vmar_allocate](../syscalls/vmar_allocate.md) - create a new child VMAR
+ [vmar_map](../syscalls/vmar_map.md) - map a VMO into a process
+ [vmar_unmap](../syscalls/vmar_unmap.md) - unmap a memory region from a process
+ [vmar_protect](../syscalls/vmar_protect.md) - adjust memory access permissions
+ [vmar_destroy](../syscalls/vmar_destroy.md) - destroy a VMAR and all of its children -->
+ [vmar_allocate](../syscalls/vmar_allocate.md) —— 创建新的子VMAR
+ [vmar_map](../syscalls/vmar_map.md) —— 将VMO映射到进程
+ [vmar_unmap](../syscalls/vmar_unmap.md) —— 从进程中取消映射的内存区域
+ [vmar_protect](../syscalls/vmar_protect.md) —— 调整内存访问权限
+ [vmar_destroy](../syscalls/vmar_destroy.md) —— 销毁VMAR及其所有子VMAR
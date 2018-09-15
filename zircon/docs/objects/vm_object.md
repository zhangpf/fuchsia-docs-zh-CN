<!-- # Virtual Memory Object -->
# Virtual Memory Object（虚拟内存对象）
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/objects/vm_object.md)

---
<!-- ## NAME -->
## 名称

<!-- vm\_object - Virtual memory containers -->
vm_object —— 虚拟内存的容器抽象

<!-- ## SYNOPSIS -->
## 概要

<!-- A Virtual Memory Object (VMO) represents a contiguous region of virtual memory
that may be mapped into multiple address spaces. -->
虚拟内存对象（VMO）表示可以映射到多个地址空间的虚拟内存连续区域。

<!-- ## DESCRIPTION -->
## 描述

<!-- VMOs are used in by the kernel and userspace to represent both paged and physical memory.
They are the standard method of sharing memory between processes, as well as between the kernel and
userspace. -->
内核和用户空间使用VMO来表示分页和物理内存。 
它们是在进程之间以及内核和用户空间之间共享内存的标准方法。

<!-- VMOs are created with [vmo_create](../syscalls/vmo_create.md) and basic I/O can be
performed on them with [vmo_read](../syscalls/vmo_read.md) and [vmo_write](../syscalls/vmo_write.md).
A VMO's size may be set using [vmo_set_size](../syscalls/vmo_set_size.md).
Conversely, [vmo_get_size](../syscalls/vmo_get_size.md) will retrieve a VMO's current size. -->
VMO通过[vmo_create](../syscalls/vmo_create.md)系统调用创建，可以使用[vmo_read](../syscalls/vmo_read.md)和[vmo_write](../syscalls/vmo_write)对它们执行基本I/O操作。 
并使用[vmo_set_size](../syscalls/vmo_set_size.md)设置VMO的大小，
相反，通过[vmo_get_size](../syscalls/vmo_get_size.md)可获取VMO当前大小。

<!-- The size of a VMO will be rounded up to the next page size boundary by the kernel. -->
VMO的大小将由内核向上舍入至下一页面边界的大小。

<!-- Pages are committed (allocated) for VMOs on demand through [vmo_read](../syscalls/vmo_read.md), [vmo_write](../syscalls/vmo_write.md), or by writing to a mapping of the VMO created using [vmar_map](../syscalls/vmar_map.md). Pages can be commited and decommited from a VMO manually by calling
[vmo_op_range](../syscalls/vmo_op_range.md) with the *ZX_VMO_OP_COMMIT* and *ZX_VMO_OP_DECOMMIT*
operations, but this should be considered a low level operation. [vmo_op_range](../syscalls/vmo_op_range.md) can also be used for cache and locking operations against pages a VMO holds. -->
通过[vmo_read](../syscalls/vmo_read.md)或[vmo_write](../syscalls/vmo_write.md)，或通过[vmar_map](../syscalls/vmar_map.md)写入VMO的映射，页面被按需提交（或分配）到VMO上。 
通过使用带*ZX_VMO_OP_COMMIT*和*ZX_VMO_OP_DECOMMIT*的标志调用[vmo_op_range](../syscalls/vmo_op_range.md)，可以手动从VMO提交和解除页面，但这被视为低层次的操作。 
[vmo_op_range](../syscalls/vmo_op_range.md)也可用于对VMO所拥有的页面进行缓存和锁操作。

<!-- Processes with special purpose use cases involving cache policy can use
[vmo_set_cache_policy](../syscalls/vmo_set_cache_policy.md) to change the policy of a given VMO.
This use case typically applies to device drivers. -->
具有涉及缓存策略的特殊使用方法的进程可以使用[vmo_set_cache_policy](../syscalls/vmo_set_cache_policy.md)来更改给定VMO的策略。 
此用法通常适用于设备驱动程序。

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [vmo_create](../syscalls/vmo_create.md) - create a new vmo
+ [vmo_read](../syscalls/vmo_read.md) - read from a vmo
+ [vmo_write](../syscalls/vmo_write.md) - write to a vmo
+ [vmo_get_size](../syscalls/vmo_get_size.md) - obtain the size of a vmo
+ [vmo_set_size](../syscalls/vmo_set_size.md) - adjust the size of a vmo
+ [vmo_op_range](../syscalls/vmo_op_range.md) - perform an operation on a range of a vmo
+ [vmo_set_cache_policy](../syscalls/vmo_set_cache_policy.md) - set the caching policy for pages held by a vmo -->
+ [vmo_create](../syscalls/vmo_create.md) —— 创建VMO
+ [vmo_read](../syscalls/vmo_read.md) —— 从VMO中读取数据
+ [vmo_write](../syscalls/vmo_write.md) —— 写入数据到VMO
+ [vmo_get_size](../syscalls/vmo_get_size.md) —— 获取VMO当前的大小
+ [vmo_set_size](../syscalls/vmo_set_size.md) —— 调整VMO的大小
+ [vmo_op_range](../syscalls/vmo_op_range.md) —— 在指定范围内的vmo上执行操作
+ [vmo_set_cache_policy](../syscalls/vmo_set_cache_policy.md) —— 为vmo拥有的页面设置缓存策略
<br>

<!-- + [vmar_map](../syscalls/vmar_map.md) - map a VMO into a process
+ [vmar_unmap](../syscalls/vmar_unmap.md) - unmap memory from a process -->

+ [vmar_map](../syscalls/vmar_map.md) —— 将VMO映射到进程中
+ [vmar_unmap](../syscalls/vmar_unmap.md) —— 从进程中取消映射内存
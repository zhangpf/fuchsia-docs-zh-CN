# Resource（资源）
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/af07ad38812f7566b6c859238ece1bb4c70b969e/docs/objects/resource.md)

---
<!-- ## NAME -->
## 名称

<!-- resource - Address space rights and accounting -->
resource —— 地址空间的权限和审计

<!-- ## SYNOPSIS -->
## 概要

<!-- A resource is an immutable object that is used to validate access to syscalls
that create objects backed by address space, or permit access to address space.
These include [vm objects](vm_object.md), [interrupts](interrupts.md), and x86
ioports. -->

资源是一个不可变对象，用于验证创建的由地址空间支持的对象的系统调用访问权，或对它们向地址空间访问权限进行授权。 
这些对象包括[vm对象](vm_object.md)，[中断](interruptts.md)和x86的ioport。

<!-- ## DESCRIPTION -->
## 描述

<!-- Resources are used to gate access to specific regions of address space and are
required to create VMOs and IRQs, as well as accessing x86 ioports. -->
资源用于控制对地址空间的特定区域的访问，并且按需要创建VMO和IRQ，以及x86上ioport的访问。

<!-- A resource object consists of a single resource *kind*, with *base* address and
*len* parameters that define a range of address space the holder of the resource
is granted access to. The range covers *base* up to but not including *base* +
*len*.  These objects are immutable after creation. Valid *kind*  values are
**ZX_RSRC_KIND_ROOT**, **ZX_RSRC_KIND_HYPERVISOR**, **ZX_RSRC_KIND_MMIO**,
**ZX_RSRC_KIND_IOPORT**, and **ZX_RSRC_KIND_IRQ**. New resources may be created
with a root resource by calling
[resource_create](../syscalls/resource_create.md). An initial root resource is
created by the kernel during boot and handed off to the first userspace process
started by userboot. -->
资源对象由单个资源*类型*组成，其中*base*和*len*参数定义了资源持有者被授予访问权限的地址空间范围，范围包括*base*，但不包括地址为*base*+*len*的位置。
这些对象在创建后是不可变的。 
有效的*种类*值为**ZX_RSRC_KIND_ROOT**，**ZX_RSRC_KIND_HYPERVISOR**，**ZX_RSRC_KIND_MMIO**，**ZX_RSRC_KIND_IOPORT**和**ZX_RSRC_KIND_IRQ**。
可以通过调用[resource_create](../syscalls/resource_create.md)使用根资源创建新的资源。 
初始根资源由内核在引导期间创建，并分发给userboot启动的第一个用户空间进程。

<!-- Resource allocations can be either *shared* or *exclusive*. A shared resource
grants the permission to access the given address space, but does not reserve
that address space exclusively for the owner of the resource. An exclusive
resource grants access to the region to only the holder of the exclusive
resource.  Exclusive and shared resource ranges may not overlap. -->
资源分配可以是*共享(shared)* 或*独占(exclusive)* 状态。 
共享资源授予访问给定地址空间的权限，但不会仅为资源所有者独占该地址空间的权限。
独占资源仅授予该区域对独占资源的持有者的访问权限。 
独占和共享资源的范围不能重叠。

<!-- Resources are lifecycle tracked and upon the last handle being closed will be
freed. In the case of exclusive resources this means the given address range
will be released back to the allocator for the given *kind* of resource. Objects
created through a resource do not hold a reference to a the resource and thus do
not keep it alive. -->
资源是生命周期可追踪的，并且在最后一个句柄关闭时被释放。 
在独占资源的情况下，这意味着给定的地址范围将被释放回给定的*种类*资源的分配器。 
通过资源创建的对象不保存对资源的引用，因此不会使其保持活动状态。

<!-- ## NOTES -->
## 注意

<!-- Resources are typically private to the DDK and platform bus drivers. Presently,
this means ACPI and platform bus hold the root resource respectively and hand out more
fine-grained resources to other drivers. -->
资源通常是DDK和平台总线驱动程序的私有资源。 
目前，这意味着ACPI和平台总线分别保存根资源，并向其他驱动程序分发更细粒度的资源。

<!-- ## SYSCALLS -->
## 系统调用

[interrupt_create](../syscalls/interrupt_create.md),
[ioports_requeat](../syscalls/ioports_request.md),
[resource_create](../syscalls/resource_create.md),
[vmo_create_physical](../syscalls/vmo_create_physical.md)

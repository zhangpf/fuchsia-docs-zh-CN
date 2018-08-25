# Bus Transaction Initiator
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fc9cc3958ceaacca9447023b8b272f6b648f75ee/docs/objects/bus_transaction_initiator.md)

---
<!-- ## NAME -->
## 名称
<!-- bus_transaction_initiator - DMA configuration capability -->
bus_transaction_initiator —— DMA配置能力

<!-- ## SYNOPSIS -->
## 概要

<!-- Bus Transaction Initiators (BTIs) represent the bus mastering/DMA capability
of a device, and can be used for granting a device access to memory. -->
总线事务启动器（BTI）表示设备的总线主控/DMA功能，可用于设备对存储器的访问进行授权。

<!-- ## DESCRIPTION -->
## 描述

<!-- Device drivers are provided one BTI for each bus transaction ID each of its
devices can use.  A bus transaction ID in this context is a hardware transaction
identifier that may be used by an IOMMU (e.g. PCI addresses on Intel's IOMMU
and StreamIDs on ARM's SMMU). -->
系统为每个设备驱动程序其对应的设备，可使用的每个总线事务ID提供一个BTI。
在此上下文中的总线事务ID是可由IOMMU使用的硬件事务标识符（例如，Intel的IOMMU上的PCI地址和ARM的SMMU上的StreamID）。

<!-- A BTI can be used to pin memory used in a Virtual Memory Object (VMO).
If a caller pins memory from a VMO, they are given device-physical addresses
that can be used to issue memory transactions to the VMO (provided the
transaction has the correct bus transaction ID).  If transactions affecting
these addresses are issued with a different transaction ID, the transaction
may fail and the issuing device may need a reset in order to continue functioning. -->
BTI可用于固定虚拟内存对象(VMO)中使用的内存。
如果调用者固定来自VMO的内存，则内核为它们提供设备物理地址，这些地址可用于向VMO发出内存事务（前提是事务具有正确的总线事务ID）。 
如果设备使用不同的事务ID发起影响这些地址的事务，则事务可能失败并且发起设备可能需要重置以便于继续运行。

<!-- A BTI manages a list of quarantined PMTs.  If a PMT was created from a BTI using
**bti_pin**(), and the PMT's handle is released without **pmt_unpin**() being
called, the PMT will be quarantined.  Quarantined PMTs will prevent their
underlying physical memory from being released to the system for reuse, in order
to prevent DMA to memory that has since been reallocated.  The quarantine may be
cleared by invoking **bti_release_quarantine**(). -->
BTI管理了隔离的PMT列表。
如果使用**bti_pin()** 从BTI创建PMT，并且在没有调用**pmt_unpin()** 的情况下释放PMT句柄，则PMT将处于隔离状态。 
隔离的PMT将阻止其底层物理内存被释放到系统以供重用，以防止DMA到这些重新分配的内存。 
可通过调用**bti_release_quarantine()** 来清除隔离内存。

<!-- TODO(teisenbe): Add details about failed transaction notification. -->
TODO(teisenbe)：添加有关失败事务通知的详细信息。

<!-- ## SEE ALSO -->
## 另见

<!-- + [pmt](pinned_memory_token.md) - Pinned Memory Tokens
+ [vm_object](vm_object.md) - Virtual Memory Objects -->
+ [pmt](pinned_memory_token.md) —— 固定内存令牌(PMT)
+ [vm_object](vm_object.md) —— 虚拟内存对象

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [bti_create](../syscalls/bti_create.md) - create a new bus transaction initiator
+ [bti_pin](../syscalls/bti_pin.md) - pin memory and grant access to it to the BTI
+ [bti_release_quarantine](../syscalls/bti_release_quarantine.md) - release quarantined PMTs
+ [pmt_unpin](../syscalls/pmt_unpin.md) - revoke access and unpin memory -->

+ [bti_create](../syscalls/bti_create.md) —— 创建新的总线事务启动器
+ [bti_pin](../syscalls/bti_pin.md) —— 固定内存并授权BTI对其的访问
+ [bti_release_quarantine](../syscalls/bti_release_quarantine.md) —— 释放隔离的PMT
+ [pmt_unpin](../syscalls/pmt_unpin.md) —— 撤消访问权限并取消固定内存 
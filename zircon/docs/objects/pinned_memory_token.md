<!-- # Pinned Memory Token -->
# 固定内存令牌 (PMT)
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fc9cc3958ceaacca9447023b8b272f6b648f75ee/docs/objects/pinned_memory_token.md)

---
<!-- ## NAME -->
## 名称

<!-- pinned_memory_token - Representation of a device DMA grant -->
pinned_memory_token —— 表示对设备DMA的授权

<!-- ## SYNOPSIS -->
## 概要
<!-- 
Pinned Memory Tokens (PMTs) represent an outstanding acccess grant to a device
for performing DMA. -->
固定内存令牌（Pinned Memory Token, 即PMT）表示对执行DMA的设备的特别访问授权。

<!-- ## DESCRIPTION -->
## 描述

<!-- PMTs are obtained by [pinning memory with a BTI object](../syscalls/bti_pin.md).
It is valid for the device associated with the BTI to access the memory represented
by the PMT for as long as the PMT object is around.  When the PMT object is
destroyed, either via **zx_handle_close**(), **zx_pmt_unpin**(), or process
termination, access to the represented memory becomes illegal (this is
enforced by hardware on systems with the capability to do so, such as IOMMUs). -->
通过[使用BTI对象固定内存](../syscalls/bti_pin.md)可获取PMT。
只要PMT对象存在，对于与BTI关联的设备访问由PMT表示的内存都是有效的。
当无论是通过**zx_handle_close()**，**zx_pmt_unpin ()**，还是进程终止导致PMT对象被销毁时，对其代表的内存的访问都变得不合法（这是由具有这种功能的系统上的硬件强制执行的，例如IOMMU）。

<!-- TODO(teisenbe): Describe quarantining -->
TODO(teisenbe)：描述隔离(quarantining)

<!-- ## SEE ALSO -->
## 另见

<!-- + [bus_transaction_initiator](bus_transaction_initiator.md) - Bus Transaction Initiators -->
+ [bus_transaction_initiator](bus_transaction_initiator.md) —— 总线事务启动器

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [bti_pin](../syscalls/bti_pin.md) - pin memory and grant access to it to the BTI
+ [pmt_unpin](../syscalls/pmt_unpin.md) - revoke access and unpin memory -->

+ [bti_pin](../syscalls/bti_pin.md) —— 固定内存并授予BTI对其的访问权限
+ [pmt_unpin](../syscalls/pmt_unpin.md) —— 撤消访问权限并取消固定内存
# zx_guest_set_trap
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/18df557635b5b32816f0236ce8ee64d38bf42188/docs/syscalls/guest_set_trap.md)

---
<!-- ## NAME -->
## 名称

<!-- guest_set_trap - sets a trap within a guest -->
guest_set_trap —— 在客户虚拟机中设置陷入中断

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/port.h>

zx_status_t zx_guest_set_trap(zx_handle_t guest, uint32_t kind, zx_vaddr_t addr,
                              size_t len, zx_handle_t port, uint64_t key);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **guest_set_trap**() sets a trap within a guest, which generates a packet when
there is an access by a VCPU within the address range defined by *addr* and
*len*, within the address space defined by *kind*. -->
**guest_set_trap()** 的功能是在客户虚拟机中设置一个陷入中断，当由*addr*和*len*定义的地址范围内的VCPU访问时，会在*kind*定义的地址空间内生成数据包。


<!-- If *port* is specified, a packet for the trap will be delivered through *port*
each time the trap is triggered, otherwise if *ZX_HANDLE_INVALID* is given, a
packet will be delivered through **vcpu_resume**(). This provides control over
whether the packet is delivered asynchronously or synchronously. -->
如果指定了*port*，在每次触发陷入时，将通过*port*传递陷入数据包，但如果指定了*ZX_HANDLE_INVALID*，则将通过**vcpu_resume()** 传递数据包。 
这提供了控制数据包是异步还是同步传送的手段。

<!-- When *port* is specified, a fixed number of packets are pre-allocated per trap.
If all the packets are exhausted, execution of the VCPU that caused the trap
will be paused. When at least one packet is dequeued, execution of the VCPU will
resume. To dequeue a packet from *port*, use **port_wait**(). Multiple threads
may use **port_wait**() to dequeue packets, enabling the use of a thread pool to
handle traps. -->
在指定*port*时，每个陷入中断会预先分配固定数量的数据包。 
如果所有数据包都已耗尽，则会暂停执行导致陷入的VCPU。 
当至少有一个数据包出队时，VCPU将恢复执行。 
要从*port*中使数据包出队，请使用**port_wait()**。 
多个线程可以使用**port_wait()** 来使数据包出列，从而允许使用线程池来处理陷入。

<!-- *key* is used to set the key field within *zx_port_packet_t*, and can be used to
distinguish between packets for different traps. -->
*key*用于设置*zx_port_packet_t*内的``key``字段，可用于区分不同类型的陷入数据包。

<!-- *kind* may be either *ZX_GUEST_TRAP_BELL*, *ZX_GUEST_TRAP_MEM*, or
*ZX_GUEST_TRAP_IO*. If *ZX_GUEST_TRAP_BELL* or *ZX_GUEST_TRAP_MEM* is specified,
then *addr* and *len* must both be page-aligned. If *ZX_GUEST_TRAP_BELL* is set,
then *port* must be specified. If *ZX_GUEST_TRAP_MEM* or *ZX_GUEST_TRAP_IO* is
set, then *port* must be *ZX_HANDLE_INVALID*. -->
*kind*的值可以是*ZX_GUEST_TRAP_BELL*，*ZX_GUEST_TRAP_MEM*或*ZX_GUEST_TRAP_IO*。
如果设置了*ZX_GUEST_TRAP_BELL*或*ZX_GUEST_TRAP_MEM*，则*addr*和*len*必须都是页对齐的。 
如果设置了*ZX_GUEST_TRAP_BELL*，则必须指定*port*。 
如果设置了*ZX_GUEST_TRAP_MEM*或*ZX_GUEST_TRAP_IO*，则*port*必须是*ZX_HANDLE_INVALID*。

<!-- *ZX_GUEST_TRAP_BELL* is a type of trap that defines a door-bell. If there is an
access to the memory region specified by the trap, then a packet is generated
that does not fetch the instruction associated with the access. The packet will
then be delivered via *port*. -->
*ZX_GUEST_TRAP_BELL*是一种定义“门铃”的陷入类型，表示如果发生对陷入指定的存储区域的访问，则生成不获取与访问相关联的指令的数据包。 
而后该数据包将通过*port*传送。

<!-- To identify what *kind* of trap generated a packet, use *ZX_PKT_TYPE_GUEST_MEM*,
*ZX_PKT_TYPE_GUEST_IO*, *ZX_PKT_TYPE_GUEST_BELL*, and *ZX_PKT_TYPE_GUEST_VCPU*.
*ZX_PKT_TYPE_GUEST_VCPU* is a special packet, not caused by a trap, that
indicates that the guest requested to start an additional VCPU. -->
要指定生成数据包的*kind*值，请使用*ZX_PKT_TYPE_GUEST_MEM*，*ZX_PKT_TYPE_GUEST_IO*，*ZX_PKT_TYPE_GUEST_BELL*或*ZX_PKT_TYPE_GUEST_VCPU*。
*ZX_PKT_TYPE_GUEST_VCPU*是一种特殊的数据包，它不是由陷入引起的，而是表示客户机请求启动另一个VCPU。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **guest_set_trap**() returns ZX_OK on success. On failure, an error value is
returned. -->
**guest_set_trap()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

## ERRORS

<!-- **ZX_ERR_ACCESS_DENIED** *guest* or *port* do not have the *ZX_RIGHT_WRITE*
right. -->
**ZX_ERR_ACCESS_DENIED**：*guest*或*port*没有*ZX_RIGHT_WRITE*权限。

<!-- **ZX_ERR_ALREADY_EXISTS** A trap with the same *kind* and *addr* already exists. -->
**ZX_ERR_ALREADY_EXISTS**：已存在具有相同*kind*和*addr*值的陷入。

<!-- **ZX_ERR_BAD_HANDLE** *guest* or *port* are invalid handles. -->
**ZX_ERR_BAD_HANDLE**：*guest*或*port*是无效句柄。

<!-- **ZX_ERR_INVALID_ARGS** *kind* is not a valid address space, *addr* or *len*
do not meet the requirements of *kind*, *len* is 0, or *ZX_GUEST_TRAP_MEM* was
specified with a *port*. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*kind*不是有效的地址空间；*addr*或*len*不符合*kind*的要求；*len*为0；或*kind*的值是*ZX_GUEST_TRAP_MEM*，但指定了*port*。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- 
**ZX_ERR_OUT_OF_RANGE** The region specified by *addr* and *len* is outside of
of the valid bounds of the address space *kind*. -->
<!-- Error!!!! -->
**ZX_ERR_OUT_OF_RANGE**： *addr*和*len*指定的内存区域超出了*kind*所允许的地址空间的有效范围。
<!-- **ZX_ERR_WRONG_TYPE** *guest* is not a handle to a guest, or *port* is not a
handle to a port. -->
**ZX_ERR_WRONG_TYPE**：*guest*不是虚拟机类型句柄，或者*port*不是端口类型句柄。

<!-- ## NOTES -->
## 注释
<!-- *ZX_GUEST_TRAP_BELL* shares the same address space as *ZX_GUEST_TRAP_MEM*. -->
*ZX_GUEST_TRAP_BELL*与*ZX_GUEST_TRAP_MEM*共享相同的地址空间。

<!-- On x86-64, if *kind* is *ZX_GUEST_TRAP_BELL* or *ZX_GUEST_TRAP_MEM* and *addr*
is the address of the local APIC, then *len* must be equivalent to the size of a
page. This is due to a special page being mapped when a trap is requested at the
address of the local APIC. This allows us to take advantage of hardware
acceleration when available. -->
在x86-64上，如果*kind*是*ZX_GUEST_TRAP_BELL*或*ZX_GUEST_TRAP_MEM*类型，且*addr*是本地APIC的地址，则*len*必须等于页面的大小。 
这是由于在本地APIC的地址发生陷入请求时需要映射的特殊页面。这也使得我们可利用硬件的加速。

<!-- ## SEE ALSO -->
## 另见

[guest_create](guest_create.md),
[port_create](port_create.md),
[port_wait](port_wait.md),
[vcpu_create](vcpu_create.md),
[vcpu_resume](vcpu_resume.md),
[vcpu_interrupt](vcpu_interrupt.md),
[vcpu_read_state](vcpu_read_state.md),
[vcpu_write_state](vcpu_write_state.md).
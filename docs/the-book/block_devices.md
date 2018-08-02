<!-- # Block Devices -->
# 块设备
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/f870f0ec91c81c83208425c865ab349abc71fd09/the-book/block_devices.md)

---
<!-- Fuchsia Block device drivers are, like other drivers on the system, implemented
as userspace services which are accessible via IPC. Programs using block devices
will have one or more handles to these underlying drivers. Similar to filesystem
clients, which may send “read” or “write” requests to servers by encoding these
requests within RPC messages, programs may act as clients to block devices, and
may transmit RPC messages to a “device host” (referred to as “devhost” within
Zircon). The devhost process then transforms these requests into
driver-understood “I/O transactions”, where they are actually transmitted to the
particular block device driver, and eventually to real hardware. -->

与Fuchsia系统上的其他驱动程序一样，块设备驱动程序以可通过IPC进行访问的用户空间服务的形式实现。使用块设备的应用程序具有一个或多个这些底层驱动程序的句柄。类似于文件系统客户端可以通过向服务发送经过编码后的“读取”或“写入”请求RPC消息的方式，同样作为块设备的客户端，程序同样可以将RPC请求消息传输到"device host"（在Zircon内简称为"devhost"）进程中。而后，devhost进程将这些请求转换为驱动程序可以理解的“I/O事务”，在这里它们实际上被传输到特定的块设备驱动程序上，并最终传输到真实的硬件。

<!-- Particular block device drivers (USB, AHCI / SATA, Ramdisk, GPT, etc) implement
the [`ZX_PROTOCOL_BLOCK_CORE`
prototol](https://fuchsia.googlesource.com/zircon/+/master/system/public/zircon/device/block.h),
which allows clients to queue transactions and query the block device. -->
特定的块设备驱动程序（USB，AHCI/SATA，Ramdisk，GPT等）均实现了[`ZX_PROTOCOL_BLOCK_CORE`协议](https://github.com/fuchsia-mirror/zircon/blob/master/system/public/zircon/device/block.h)，该协议允许客户端对事务进行排队以及对块设备进行查询。

<!-- ## Fast Block I/O -->
## 快速块设备I/O

<!-- Block device drivers are often responsible for taking large portions of memory,
and queueing requests to a particular device to either “read into” or “write
from” a portion of memory. Unfortunately, as a consequence of transmitting
messages of a limited size from an RPC protocol into an “I/O transaction”,
repeated copying of large buffers is often required to access block devices. -->
块设备驱动程序通常负责处理大块内存区域，并将请求排队到特定设备以“读入”或“写入”一部分内存数据。遗憾的是，由于需要将有大小限制的消息从RPC协议发送到“I/O事务”，这通常需要重复复制大量缓冲区来访问块设备。

<!-- To avoid this performance bottleneck, the block device drivers implement
another mechanism to transmit reads and writes: a fast, FIFO-based protocol
which acts on a shared VMO. Filesystems (or any other client wishing to
interact with a block device) can acquire FIFOs from a block device, register a
“transaction buffer”, and pass handles to VMOs to the block device. Instead of
transmitting “read” or “write” messages with large buffers, a client of this
protocol can instead send a fast, lightweight control message on a FIFO,
indicating that the block device driver should act directly on the
already-registered VMO. For example, when writing to a file, rather than
passing bytes over IPC primitives directly, and copying them to a new location
in the block device’s memory, a filesystem (representing the file as a VMO)
could simply send a small FIFO message indicating “write N bytes directly from
offset X of VMO Y to offset Z on a disk”. When combined with the “mmap”
memory-mapping tools, this provides a “zero-copy” pathway directly from client
programs to disk (or in the other direction) when accessing files. -->
为了避免这种性能瓶颈，块设备驱动程序实现了另一种传输读写的机制：作用于共享的[VMO](/zircon/docs/concepts.md#共享内存-虚拟内存对象vmo)之上的，基于FIFO的快速协议。文件系统（或任何其他希望与块设备交互的客户端）可以从块设备获取FIFO，注册“事务缓冲区”，并将句柄传递给块设备。该协议的客户端可以在FIFO上发送快速轻量级的控制消息，而不是使用大缓冲区发送“读取”或“写入”消息，这表明块设备驱动程序是直接作用于已经注册的VMO上的。举个例子说明，当写入文件时，不是直接在IPC原语上传递字节，而是将它们复制到块设备内存中的新区域，使得文件系统（将文件表示为VMO）可以简单地发送一个小的FIFO消息到驱动，表明“直接从VMO Y，偏移量X位置写入N个字节到磁盘上的偏移量Z位置“。进一步地，当与"mmap"内存映射工具结合使用时，在访问文件时，为从客户端程序到磁盘（或相反方向）提供直接“零拷贝”的路径。
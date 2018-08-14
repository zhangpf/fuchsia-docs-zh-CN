# FIFO
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/objects/fifo.md)

---
<!-- ## NAME -->
## 名称

<!-- FIFO - first-in first-out interprocess queue -->
FIFO——先入先出的进程间通信队列

<!-- ## SYNOPSIS -->
## 概要

<!-- FIFOs are intended to be the control plane for shared memory
transports.  Their read and write operations are more efficient than
[sockets](socket.md) or [channels](channel.md), but there are severe
restrictions on the size of elements and buffers. -->

FIFO的目的是成为共享内存传输的控制面，它们的读写操作比[socket](socket.md)或[channel](channel.md)更高效，但对元素和缓冲区的大小有严格的限制。

<!-- ## DESCRIPTION -->
## 描述

TODO

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [fifo_create](../syscalls/fifo_create.md) - create a new fifo
+ [fifo_read](../syscalls/fifo_read.md) - read data from a fifo
+ [fifo_write](../syscalls/fifo_write.md) - write data to a fifo -->

+ [fifo_create](../syscalls/fifo_create.md) —— 创建fifo
+ [fifo_read](../syscalls/fifo_read.md) —— 从fifo中读取数据
+ [fifo_write](../syscalls/fifo_write.md) —— 写入数据到fifo中
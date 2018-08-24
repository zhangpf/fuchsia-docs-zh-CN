# Port
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/d17af78df889107ed5035b3f420567675a3c6ee5/docs/objects/port.md)

---
<!-- ## NAME -->
## 名称

<!-- port - Signaling and mailbox primitive -->
port —— 信号和邮箱(mailbox)原语

<!-- ## SYNOPSIS -->
## 概要

<!-- Ports allow threads to wait for packets to be delivered from various
events. These events include explicit queueing on the port,
asynchronous waits on other handles bound to the port, and
asynchronous message delivery from IPC transports. -->
端口(port)允许线程等待从各种事件对象传递的数据包。这些事件包括port上的显式排队，绑定到端口的其他句柄上的异步等待以及来自IPC的异步消息传递。

<!-- ## DESCRIPTION -->
## 描述

TODO

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [port_create](../syscalls/port_create.md) - create a port
+ [port_queue](../syscalls/port_queue.md) - send a packet to a port
+ [port_wait](../syscalls/port_wait.md) - wait for packets to arrive on a port -->

+ [port_create](../syscalls/port_create.md) —— 创建端口
+ [port_queue](../syscalls/port_queue.md) —— 发送数据包到端口
+ [port_wait](../syscalls/port_wait.md) —— 等待数据包到达端口

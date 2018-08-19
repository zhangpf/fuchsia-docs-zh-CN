# Event Pair
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/objects/eventpair.md)

---
<!-- ## NAME -->
## 名称
<!-- 
eventpair - Mutually signalable pair of events for concurrent programming -->
eventpair —— 用于并发编程的可相互通知的事件对

<!-- ## SYNOPSIS -->
## 概要

<!-- Event Pairs are linked pairs of user-signalable objects. The 8 signal
bits reserved for userspace (*ZX_USER_SIGNAL_0* through
*ZX_USER_SIGNAL_7*) may be set or cleared on the local or opposing
endpoint of an Event Pair. -->
Event pair是相互连接的用户可通知的一对对象，为用户空间保留的8个信号位（*ZX_USER_SIGNAL_0*至*ZX_USER_SIGNAL_7*）可以用于设置或清除本地或Event Pair相对端点上的信号。

<!-- ## DESCRIPTION -->
## 描述

TODO

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [eventpair_create](../syscalls/eventpair_create.md) - create a connected pair of events -->
+ [eventpair_create](../syscalls/eventpair_create.md) —— 创建一对相互连接的事件

<br>

<!-- + [object_signal_peer](../syscalls/object_signal.md) - set or clear the user signals in the opposite end -->

+ [object_signal_peer](../syscalls/object_signal.md) —— 在相对端点上设置或清除用户信号
# Event
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/objects/event.md)

---
<!-- ## NAME -->
## 名称

<!-- event - Signalable event for concurrent programming -->
event —— 并发编程中可通知的event对象

<!-- ## SYNOPSIS -->
## 概要

<!-- Events are user-signalable objects. The 8 signal bits reserved for
userspace (*ZX_USER_SIGNAL_0* through *ZX_USER_SIGNAL_7*) may be set,
cleared, and waited upon. -->
Event是可通知用户的对象，为用户空间保留的8个信号位（*ZX_USER_SIGNAL_0*至*ZX_USER_SIGNAL_7*）可以用于设置，清除和等待事件。

<!-- ## DESCRIPTION -->
## 描述

TODO

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [event_create](../syscalls/event_create.md) - create an event -->
+ [event_create](../syscalls/event_create.md) —— 创建event

<!-- + [object_signal](../syscalls/object_signal.md) - set or clear the user signals on an object -->
+ [object_signal](../syscalls/object_signal.md) —— 设置或清除对象上的用户信号

<!-- ## SEE ALSO -->
## 另见

<!-- + [eventpair](eventpair.md) - linked pairs of signalable objects -->
+ [eventpair](eventpair.md) —— 连接的可互通知的的一对事件对象

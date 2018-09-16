# Interrupts
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fd7c237ca14f48dd4f1d7a0a7c3d2d3d304dbb47/docs/objects/interrupts.md)

---

<!-- ## NAME -->
## 名称

<!-- interrupts - Usermode I/O interrupt delivery -->
interrupt —— 用户模式的I/O中断传递


<!-- ## SYNOPSIS -->
## 概要

<!-- Interrupt objects allow userspace to create, signal, and wait on
hardware interrupts. -->
中断对象允许用户空间创建、发送信号以及等待硬件中断。

<!-- ## DESCRIPTION -->
## 描述

TODO

<!-- ## NOTES -->
## 注释
<!-- 
Interrupt Objects are private to the DDK and not generally available
to userspace processes. -->
中断对象仅供DDK专用，通常不提供给用户空间进程使用。

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [interrupt_create](../syscalls/interrupt_create.md) - Create an interrupt handle
+ [interrupt_bind](../syscalls/interrupt_bind.md) - Bind an interrupt vector to interrupt handle
+ [interrupt_wait](../syscalls/interrupt_wait.md) - Wait for an interrupt on an interrupt handle
+ [interrupt_get_timestamp](../syscalls/interrupt_get_timestamp.md) - Get the timestamp for an interrupt
+ [interrupt_signal](../syscalls/interrupt_signal.md) - Signals a virtual interrupt on an interrupt handle -->

+ [interrupt_create](../syscalls/interrupt_create.md) —— 创建中断对象
+ [interrupt_bind](../syscalls/interrupt_bind.md) —— 绑定中断向量到中断对象句柄
+ [interrupt_wait](../syscalls/interrupt_wait.md) —— 等待中断对象句柄产生中断
+ [interrupt_ack](../syscalls/interrupt_ack.md) —— 确认应答并重新启动中断。
+ [interrupt_destroy](../syscalls/interrupt_destroy.md) —— 销毁中断对象
+ [interrupt_trigger](../syscalls/interrupt_trigger.md) —— 触发虚拟中断对象
# Task（任务）
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/af07ad38812f7566b6c859238ece1bb4c70b969e/docs/objects/task.md)

---
<!-- ## NAME -->
## 名称
<!-- 
Task - "Runnable" subclass of kernel objects (threads, processes, and jobs) -->
任务 —— 内核对象中的“可运行”子类（包括线程，进程和作业）

<!-- ## SYNOPSIS -->
## 概要

<!-- [Threads](thread.md), [processes](process.md), and [jobs](job.md) objects
are all tasks. They share the ability to be suspended, resumed, and
killed. -->
[线程](thread.md)，[进程](process.md)和[作业](job.md)对象都是任务类型。 
他们都具有被挂起，恢复和终止的能力。

<!-- ## DESCRIPTION -->
## 描述

TODO

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [task_resume](../syscalls/task_resume.md) - cause a suspended task to continue running
+ [task_bind_exception_port](../syscalls/task_bind_exception_port.md) - attach an exception port to a task
+ [task_kill](../syscalls/task_kill.md) - cause a task to stop running -->

+ [task_resume](../syscalls/task_resume.md) —— 继续运行已挂起的任务
+ [task_bind_exception_port](../syscalls/task_bind_exception_port.md) —— 在任务上挂载异常端口
+ [task_kill](../syscalls/task_kill.md) —— 停止任务的运行
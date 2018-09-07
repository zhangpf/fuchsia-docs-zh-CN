# Job（作业）
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/a73de2ea77ae17a6b2e57286299e57cc2b838dc7/docs/objects/job.md)

---
<!-- ## NAME -->
## 名称

<!-- job - Control a group of processes -->
job —— 控制一组进程

<!-- ## SYNOPSIS -->
## 概要

<!-- A job is a group of processes and possibly other (child) jobs. Jobs are used to
track privileges to perform kernel operations (i.e., make various syscalls,
with various options), and track and limit basic resource (e.g., memory, CPU)
consumption. Every process belongs to a single job. Jobs can also be nested,
and every job except the root job also belongs to a single (parent) job. -->
作业是一组进程，以及其它可能的（子）作业的集合。 
作业用于追踪用于执行内核操作的权限（即使用各种选项进行各种系统调用），并追踪和限制基本资源（例如，内存和CPU）的消耗。 
每个进程都属于一个作业中。 
作业也可以相互嵌套，除根作业之外的每个作业也属于某个单个（父）作业。

<!-- ## DESCRIPTION -->
## 描述

<!-- A job is an object consisting of the following: -->
作业是由以下内容组成的对象：
<!-- + a reference to a parent job
+ a set of child jobs (each of whom has this job as parent)
+ a set of member [processes](process.md)
+ a set of policies [⚠ not implemented] -->
+ 指向父作业的引用
+ 一组子作业（每个子作业都以本作业作为其父作业）
+ 一组[进程](process.md)
+ 一组策略[⚠未实现]

<!-- Jobs control "applications" that are composed of more than one process to be
controlled as a single entity. -->
作业控制“应用程序”由作为单个项被控制的多个进程所组成。

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [job_create](../syscalls/job_create.md) - create a new child job.
+ [process_create](../syscalls/process_create.md) - create a new process
  within a job.
+ [job_set_policy](../syscalls/job_set_policy.md) - set policy for
  new processes in the job.
+ [task_resume](../syscalls/task_resume.md) - cause a suspended task to
  continue running.
+ [task_bind_exception_port](../syscalls/task_bind_exception_port.md) -
  attach an exception port to a task
+ [task_kill](../syscalls/task_kill.md) - cause a task to stop running. -->

+ [job_create](../syscalls/job_create.md) —— 创造新的子作业。
+ [process_create](../syscalls/process_create.md) —— 在作业中创建新进程。
+ [job_set_policy](../syscalls/job_set_policy.md) —— 为作业中的新进程设置策略。
+ [task_resume](../syscalls/task_resume.md) —— 触发暂停的任务(task)继续运行。
+ [task_bind_exception_port](../syscalls/task_bind_exception_port.md) —— 附加异常端口到一个任务上。
+ [task_kill](../syscalls/task_kill.md) —— 触发任务的暂停。
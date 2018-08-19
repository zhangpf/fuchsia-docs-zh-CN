# Log
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/objects/log.md)

---
<!-- ## NAME -->
## 名称

<!-- Log - Kernel debug log -->
Log —— 内核调试日志

<!-- ## SYNOPSIS -->
## 概览
<!-- 
Log objects allow userspace to read and write to kernel debug logs. -->
Log对象允许用户空间读取和写入内核调试日志。

<!-- ## DESCRIPTION -->
## 描述

TODO

<!-- ## NOTES -->
## 注意

<!-- Log objects will likely cease being generally available to userspace
processes in the future.  They are intended for internal logging of
the kernel and device drivers. -->
日志对象将来可能会停止供用户空间进程通用使用。因为设计它们本意是，为内核和设备驱动程序提供内部日志记录。

<!-- ## SYSCALLS -->
## 系统调用
<!-- 
+ log_create - create a kernel managed log reader or writer
+ log_write - write log entry to log
+ log_read - read log entries from log -->

+ log_create —— 创建内核管理的日志读写对象
+ log_write —— 写入日志项到log中
+ log_read —— 从log中读取日志项

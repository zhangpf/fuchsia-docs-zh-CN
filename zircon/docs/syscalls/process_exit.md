# zx_process_exit
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/c90f6a56c60f0484be9f610096fb8d58edfef424/docs/syscalls/process_exit.md)

---
<!-- ## NAME -->
## 名称

<!-- process_exit - Exits the currently running process. -->
process_exit —— 退出当前运行中的进程。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

void zx_process_exit(int ret_code);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- The **zx_process_exit** call ends the calling process with the given
return code. The return code of a process can be queried via the
**ZX_INFO_PROCESS** request to **zx_object_get_info**. -->
**zx_process_exit**的功能是结束调用进程，以给定的值作为返回码。 
通过以**ZX_INFO_PROCESS**为参数的**zx_object_get_info**系统调用，可以查询进程的返回码。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_process_exit** does not return. -->
**zx_process_exit**不再返回。

<!-- ## ERRORS -->
## 错误码

<!-- **zx_process_exit** cannot fail. -->
**zx_process_exit**不会执行失败。

<!-- ## SEE ALSO -->
## 另见

[object_get_info](object_get_info.md),
[process_create](process_create.md).
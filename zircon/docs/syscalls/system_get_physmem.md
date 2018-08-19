# zx_system_get_physmem
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/system_get_physmem.md)

---
<!-- ## NAME -->
## 名称

<!-- system_get_physmem - get amount of physical memory on the system -->
system_get_physmem —— 获取系统中的物理内存总量


<!-- ## SYNOPSIS -->
## 概览

```
#include <zircon/syscalls.h>

size_t zx_system_get_physmem(void);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **system_get_physmem**() returns the total size of physical memory on
the machine, in bytes. -->
**system_get_physmem()** 返回机器上物理内存的总量（以字节为单位）。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **system_get_physmem**() returns a number in bytes. -->
**system_get_physmem()** 返回内存（以字节计数）的总量。


<!-- ## ERRORS -->
## 错误码

<!-- **system_get_physmem**() cannot fail. -->
**system_get_physmem()** 不会失败。

<!-- ## NOTES -->
## 注意

<!-- Currently the total size of physical memory cannot change during a run of
the system, only at boot time.  This might change in the future. -->
目前，此数字只有在启动阶段才可能改变，但可能在将来会发生变化。

<!-- ## SEE ALSO -->
## 另见

<!-- [system_get_num_cpus](system_get_num_cpus.md). -->
[system_get_num_cpus](system_get_num_cpus.md)。

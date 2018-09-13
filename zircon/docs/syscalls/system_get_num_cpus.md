# zx_system_num_cpus
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/system_get_num_cpus.md)

---

<!-- ## NAME -->
## 名称

<!-- system_get_num_cpus - get number of logical processors on the system -->
system_get_num_cpus —— 获取系统中的逻辑处理器的数量 

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

uint32_t zx_system_get_num_cpus(void);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **system_get_num_cpus**() returns the number of CPUs (logical processors)
that exist on the system currently running.  This number cannot change
during a run of the system, only at boot time. -->
**system_get_num_cpus()** 返回当前正在运行的系统上的CPU（逻辑处理器）数量，此数字只有在启动阶段才可能发生改变。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **system_get_num_cpus**() returns the number of CPUs. -->
**system_get_num_cpus()** 返回CPU的数量。

<!-- ## ERRORS -->
## 错误码

**system_get_num_cpus()** 不会产生失败。

<!-- ## NOTES -->
## 注意

<!-- ## SEE ALSO -->
## 另见

<!-- [system_get_physmem](system_get_physmem.md). -->
[system_get_physmem](system_get_physmem.md)。
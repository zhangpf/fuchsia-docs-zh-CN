# zx_system_get_version
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/system_get_version.md)

---
<!-- ## NAME -->
## 名称

<!-- system_get_version - get version string for system -->
system_get_version —— 获取系统版本字符串

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_system_get_version(char version[], size_t version_size);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **system_get_version**() fills in the given character array with a string
identifying the version of the Zircon system currently running.
The provided size must be large enough for the complete string
including its null terminator. -->
**system_get_version()** 用标识当前运行的Zircon系统的版本的字符串填充给定的字符数组。所提供的字符串的大小必须足够大，以包含完整的字符串，包括null终止符。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **system_get_version**() returns **ZX_OK** on success. -->
**system_get_version()** 执行成功则返回**ZX_OK**。
<!-- ## ERRORS -->
## 错误码
<!-- 
**ZX_ERR_BUFFER_TOO_SMALL**  *version_size* is too short. -->
**ZX_ERR_BUFFER_TOO_SMALL**：*version_size*长度太短。

<!-- ## NOTES -->
## 注意

<!-- ## SEE ALSO -->
## 另见

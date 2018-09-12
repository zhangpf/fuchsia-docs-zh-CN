# zx_cprng_add_entropy
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/b1ee78419ac2dc207a2f5b2e8fc69fa56101df90/docs/syscalls/cprng_add_entropy.md)

---
<!-- ## NAME -->
## 名称

<!-- zx_cprng_add_entropy - Add entropy to the kernel CPRNG -->
zx_cprng_add_entropy —— 将熵添加到内核CPRNG中

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_cprng_add_entropy(const void* buffer, size_t buffer_size);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_cprng_add_entropy**() mixes the given entropy into the kernel CPRNG.
a privileged operation.  It will accept at most **ZX_CPRNG_ADD_ENTROPY_MAX_LEN**
bytes of entropy at a time. -->
**zx_cprng_add_entropy()** 是特权操作，它将给定的熵混合到内核CPRNG中。 
它一次最多只接受**ZX_CPRNG_ADD_ENTROPY_MAX_LEN**个字节的熵。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_cprng_add_entropy**() returns **ZX_OK** on success. -->
**zx_cprng_add_entropy()** 调用成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS** *buffer_size* is too large, or *buffer* is not a valid
userspace pointer. -->
**ZX_ERR_INVALID_ARGS**：*buffer_size*太大，或者*buffer*不是有效的用户空间指针。

## BUGS

<!-- This syscall should be very privileged. -->
该系统调用需要特别高的权限。
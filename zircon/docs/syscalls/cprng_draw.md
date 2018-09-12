# zx_cprng_draw
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/b1ee78419ac2dc207a2f5b2e8fc69fa56101df90/docs/syscalls/cprng_add_entropy.md)

---
<!-- ## NAME -->
## 名称

<!-- zx_cprng_draw - Draw from the kernel's CPRNG -->
zx_cprng_draw —— 从内核的CPRNG中提取随机数据

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

void zx_cprng_draw(void* buffer, size_t buffer_size);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_cprng_draw**() draws random bytes from the kernel CPRNG.  This data should
be suitable for cryptographic applications. -->
**zx_cprng_draw()** 的功能是从内核CPRNG中抽取随机字节。 
这些随机数据适用于加密应用程序。

<!-- Clients that require a large volume of randomness should consider using these
bytes to seed a user-space random number generator for better performance. -->
需要大量随机数的客户端应考虑使用这些字节，来为用户空间随机数生成器设定种子，以获得更好的性能。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## NOTES -->
## 注意

<!-- **zx_cprng_draw**() triggers terminates the calling process if **buffer** is not
a valid userspace pointer. -->
如果**buffer**不是有效的用户空间指针，则**zx_cprng_draw()**j将触发调用进程的终止。

<!-- There are no other error conditions.  If its arguments are valid,
**zx_cprng_draw**() will succeed. -->
除此之外，没有其它的错误条件。 
如果它的参数都是有效的，则**zx_cprng_draw()** 将调用成功。
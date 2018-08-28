# zx_cache_flush
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/a9ff88e1fd77ac9146e98980080a246418057ad5/docs/syscalls/cache_flush.md
)

---
<!-- ## NAME -->
## 名称

<!-- zx_cache_flush - Flush CPU data and/or instruction caches -->
zx_cache_flush —— 刷新CPU数据和/或指令缓存

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_cache_flush(const void* addr, size_t size, uint32_t flags);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_cache_flush**() flushes CPU caches covering memory in the given
virtual address range.  If that range of memory is not readable, then
the thread may fault as it would for a data read. -->
**zx_cache_flush()** 刷新覆盖给定虚拟地址范围内存的CPU缓存。
如果该范围内存不可读，则线程可能会像其他数据读取一样发生故障。

<!-- *flags* is a bitwise OR of: -->
*flags*是如下的标志位按位取或：

 * **ZX_CACHE_FLUSH_DATA**

   <!-- Clean (write back) data caches, so previous writes on this CPU are
   visible in main memory. -->
   清理（写回）数据缓存，使得在此CPU先前的写入数据在主存中可见。

 <!-- * **ZX_CACHE_FLUSH_INVALIDATE**
   (valid only when combined with **ZX_CACHE_FLUSH_DATA**) -->
   

   <!-- Clean (write back) data caches and then invalidate data caches, so
   previous writes on this CPU are visible in main memory and future
   reads on this CPU see external changes to main memory. -->
 * **ZX_CACHE_FLUSH_INVALIDATE**（仅与**ZX_CACHE_FLUSH_DATA**组合时有效）
    
    清理（写回）数据缓存，然后使数据缓存无效，使得在此CPU先前的写入数据在主存中可见，并且未来在此CPU上的读取将看到对主存的外部更改。

 * **ZX_CACHE_FLUSH_INSN**

   <!-- Synchronize instruction caches with data caches, so previous writes
   on this CPU are visible to instruction fetches.  If this is combined
   with **ZX_CACHE_FLUSH_DATA**, then previous writes will be visible to
   main memory as well as to instruction fetches. -->
   同步指令缓存与数据缓存，使得在该CPU上的先前写入对取指令可见。 
   如果将其与**ZX_CACHE_FLUSH_DATA**组合使用，则先前的写操作将对主存以及取指令可见。
<!-- 
At least one of **ZX_CACHE_FLUSH_DATA** and **ZX_CACHE_FLUSH_INSN**
must be included in *flags*. -->
**ZX_CACHE_FLUSH_DATA**和**ZX_CACHE_FLUSH_INSN**标志位必须至少有一个包含在*flags*中。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_cache_flush**() returns **ZX_OK** on success, or an error code on failure. -->
**zx_cache_flush()** 调用成功则返回**ZX_OK**，失败时返回错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS** *flags* is invalid. -->
**ZX_ERR_INVALID_ARGS**：*flags*无效。
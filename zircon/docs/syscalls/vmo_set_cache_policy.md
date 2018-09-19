# zx_set_cache_policy
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_set_cache_policy.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_set_cache_policy - set the caching policy for pages held by a VMO. -->
vmo_set_cache_policy —— 为VMO持有的页面设置缓存策略。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t vmo_set_cache_policy(zx_handle_t handle, uint32_t cache_policy);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmo_set_cache_policy()** sets caching policy for a VMO. Generally used on VMOs
that point directly at physical memory. Such VMOs are generally only handed to
userspace via bus protocol interfaces, so this syscall will typically only be
used by drivers dealing with device memory. This call can also be used on a
regular memory backed VMO with similar limitations and uses. -->
**vmo_set_cache_policy()** 的功能是为VMO设置缓存策略，通常用于直接引用物理内存的VMO。 
这样的VMO通常只通过总线协议接口传递到用户空间中，因此该系统调用通常仅由处理设备内存的驱动程序使用。 
不过它也可以在具有类似限制和用途的，指向常规内存的VMO上使用。

<!-- A handle must have the *ZX_RIGHT_MAP* right for this call to be
permitted. Additionally, the VMO must not presently be mapped by any process,
be cloned, be a clone itself, or have any memory committed. -->
句柄必须具有*ZX_RIGHT_MAP*权限才可进行此调用。 
此外，此调用的VMO目前也不能被任何进程映射、被克隆、成为自身的副本或有任何内存的提交。

<!-- *cache_policy* cache flags to use: -->
*cache_policy*可使用的缓存标志位：

<!-- **ZX_CACHE_POLICY_CACHED** - Use hardware caching. -->
**ZX_CACHE_POLICY_CACHED** —— 使用硬件缓存。

<!-- **ZX_CACHE_POLICY_UNCACHED** - Disable caching. -->
**ZX_CACHE_POLICY_UNCACHED** —— 禁用缓存。

<!-- **ZX_CACHE_POLICY_UNCACHED_DEVICE** - Disable cache and treat as device memory.
This is architecture dependent and may be equivalent to
*ZX_CACHE_POLICY_UNCACHED* on some architectures. -->
**ZX_CACHE_POLICY_UNCACHED_DEVICE** —— 禁用缓存并将其视为设备内存。 
该标志位的含义取决于体系结构，在某些体系结构上可能等同于*ZX_CACHE_POLICY_UNCACHED*。

<!-- **ZX_CACHE_POLICY_WRITE_COMBINING** - Uncached with write combining. -->
**ZX_CACHE_POLICY_WRITE_COMBINING** —— 使用合并写策略进行缓存提交。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmo_set_cache_policy()** returns **ZX_OK** on success. In the event of
failure, a negative error value is returned. -->
**vmo_set_cache_policy()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码
<!-- 
**ZX_ERR_ACCESS_DENIED** Cache policy has been configured for this VMO already and
may not be changed, or *handle* lacks the ZX_RIGHT_MAP right. -->
**ZX_ERR_ACCESS_DENIED**：之前已为此VMO配置了缓存策略，故可能无法更改，或*handle*缺少*ZX_RIGHT_MAP*权限。

<!-- **ZX_ERR_BAD_HANDLE** *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_INVALID_ARGS** *cache_policy* contains flags outside of the ones listed
above, or *cache_policy* contains an invalid mix of cache policy flags. -->
**ZX_ERR_INVALID_ARGS**：*cache_policy*包含上面列出的标志位之外的标志，或*cache_policy*包含缓存策略标志的无效组合。

<!-- **ZX_ERR_NOT_SUPPORTED** The VMO *handle* corresponds to is not one holding
physical memory. -->
**ZX_ERR_NOT_SUPPORTED**：VMO的句柄*handle*对应的不是一个持有物理内存的对象。

<!-- **ZX_ERR_BAD_STATE** Cache policy cannot be changed because the VMO is presently
mapped, cloned, a clone itself, or have any memory committed. -->
**ZX_ERR_BAD_STATE**：无法更改缓存策略，因为VMO目前有下列情况之一：被进程映射；被克隆；成为自身的副本；或有内存的提交。

<!-- ## SEE ALSO -->
## 另见

[vmo_create](vmo_create.md), [vmo_read](vmo_read.md), [vmo_write](vmo_write.md),
[vmo_get_size](vmo_get_size.md), [vmo_set_size](vmo_set_size.md),
[vmo_op_range](vmo_op_range.md).
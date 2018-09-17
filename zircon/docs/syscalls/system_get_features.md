# zx_system_get_features
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/system_get_features.md)

---

<!-- ## NAME -->
## 名称

<!-- system_get_features - get supported hardware capabilities -->
system_get_features —— 获取支持的硬件功能

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_system_features(uint32_t kind, uint32_t* features);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- 
**system_get_features**() populates *features* with a bit mask of
hardware-specific features.  *kind* indicates the specific type of features
to retrieve, e.g. *ZX_FEATURE_KIND_CPU*.  The supported kinds and the meaning
of individual feature bits is hardware-dependent. -->
**system_get_features()** 使用位掩码来填充*features*值，以表示硬件特定功能。*kind*表示要查询的特定功能类型，如*ZX_FEATURE_KIND_CPU*。支持的类型和各个特征位的含义取决于硬件。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **system_get_features**()  returns **ZX_OK** on success. -->
**system_get_features()** 在执行成功时返回**ZX_OK**.

<!-- ## ERRORS -->
## 错误码
<!-- 
**ZX_ERR_NOT_SUPPORTED**  The requested feature kind is not available on this
platform. -->
**ZX_ERR_NOT_SUPPORTED**：此平台不提供所请求的功能类型。

<!-- ## NOTES -->
## 注释

<!-- Refer to [Architecture Support](../architecture_support.md) for supported
processor architectures. -->
关于支持的处理器体系结构，请参阅[体系结构支持](../architecture_support.md)。

<!-- Refer to [zircon/features.h](../../system/public/zircon/features.h) for kinds
of features and individual feature bits. -->
有关各种功能和各个功能位，请参阅[zircon/ features.h](https://github.com/fuchsia-mirror/zircon/blob/master/system/public/zircon/features.h)。

<!-- ## SEE ALSO -->
## 另见

<!-- [system_get_num_cpus](system_get_num_cpus.md)
[system_get_physmem](system_get_physmem.md) -->
[system_get_num_cpus](system_get_num_cpus.md)，[system_get_physmem](system_get_physmem.md)。
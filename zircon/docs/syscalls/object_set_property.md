# zx_object_set_property
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_set_property.md)

---
<!-- ## NAME -->
## 名称

<!-- object_set_property - Set various properties of various kernel objects. -->
object_set_property —— 设置内核对象的各种属性值。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_object_set_property(zx_handle_t handle, uint32_t property,
                                   const void* value, size_t value_size);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- See [object_get_property()](object_get_property.md) for a full description. -->
有关该调用完整的描述，请参见[object_get_property()](object_get_property.md)。
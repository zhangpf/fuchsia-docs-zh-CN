# zx_object_get_child
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_get_child.md)

---
<!-- ## NAME -->
## 名称

<!-- object_get_child - Given a kernel object with children objects, obtain
a handle to the child specified by the provided kernel object id. -->
object_get_child —— 给定具有子对象的内核对象，获取由内核对象id指定的子对象的句柄。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_object_get_child(zx_handle_t handle, uint64_t koid,
                                zx_rights_t rights, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_object_get_child** attempts to find a child of the object referred to
by *handle* which has the kernel object id specified by *koid*.  If such an
object exists, and the requested *rights* are not greater than those provided
by the *handle* to the parent, a new handle to the specified child object is
returned. -->
**zx_object_get_child** 的功能是查找由*handle*引用的对象的子对象，该对象具有由*koid*指定的内核对象id。 
如果存在该对象，并且请求的权限*rights*不超过由*handle*指定的父对象的权限，则返回指定子对象的新句柄。

<!-- *rights* may be *ZX_RIGHT_SAME_RIGHTS* which will result in rights equivalent
to the those on the *handle*. -->
*rights*可以是*ZX_RIGHT_SAME_RIGHTS*，它表示权限等同于*handle*上的权限。

<!-- If the object is a *Process*, the *Threads* it contains may be obtained by
this call. -->
如果对象是*进程*类型，则可以通过此调用获得它包含的*线程*。

<!-- If the object is a *Job*, its (immediate) child *Jobs* and the *Processes*
it contains may be obtained by this call. -->
如果对象是*任务*类型，则可以通过此调用获取它的（直接）子*任务*及其包含的*进程*。

<!-- If the object is a *Resource*, its (immediate) child *Resources* may be
obtained by this call. -->
如果对象是*资源*类型，则可以通过此调用获取它的（直接）子*资源*。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- On success, **ZX_OK** is returned and a handle to the desired child object is returned via *out*. -->
调用成功则返回**ZX_OK**，并通过*out*返回获取的子对象的句柄。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a *Process*, *Job*, or *Resource*. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是*进程*，*任务*或*资源*类型的句柄。

<!-- **ZX_ERR_ACCESS_DENIED**   *handle* lacks the right **ZX_RIGHT_ENUMERATE** or *rights* specifies
rights that are not present on *handle*. -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少**ZX_RIGHT_ENUMERATE**权限，或*rights*指定了*handle*上不存在的权限。

<!-- **ZX_ERR_NOT_FOUND**  *handle* does not have a child with the kernel object id *koid*. -->
**ZX_ERR_NOT_FOUND**：*handle*没有id为*koid*的内核子对象。

<!-- 
**ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_INVALID_ARGS**  *out* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*out*是无效指针。

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[handle_replace](handle_replace.md),
[object_get_info](object_get_info.md).
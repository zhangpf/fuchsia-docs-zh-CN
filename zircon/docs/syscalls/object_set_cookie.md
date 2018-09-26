# zx_object_set_cookie
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_set_cookie.md)

---
<!-- ## NAME -->
## 名称

<!-- object_set_cookie - Set an object's cookie. -->
object_set_cookie —— 设置对象的cookie值。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_object_set_cookie(zx_handle_t handle, zx_handle_t scope, uint64_t cookie);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- Some objects (Events, Event pairs, Resources, VMOs) may have a Cookie attached,
which is a 64bit opaque value.  Initially the Cookie is undefined and not
readable. -->
某些类型的对象（事件，事件对，资源和VMO）可能会附加一个cookie，这是一个64位的不透明值。 
在对象新创建时，cookie是未定义且不可读的。

<!-- Once **zx_object_set_cookie**() is called successfully, the cookie is set,
and the Object referenced by the *scope* handle becomes the key necessary
to read the cookie or modify it.  The *scope* may never be changed for the
lifetime of the object. -->
成功调用**zx_object_set_cookie()** 后，cookie的值被修改，并且*scope*句柄引用的对象将成为读取或修改cookie所必需的键值。 
*scope*在对象的生命周期内不可被更改。

<!-- Event pairs are special.  If one side of the pair is closed, the other side's
cookie is invalidated. An invalidated cookie is not get-able or set-able with any scope. -->
事件对在cookie方面有些特别。 
如果事件对的一端关闭，则另一端的cookie也将失效。 
失效的cookie无法使用任何范围(scope)来获取或设置值。

<!-- Cookies are useful for objects that will be passed to another process and
later returned.  By setting the cookie with **zx_object_set_cookie**(),
using a *scope* that is not accessible by other processes, **zx_object_get_cookie**()
may later be used to verify that a handle is referring to an object that was
"created" by the calling process and simultaneously return an ID or pointer
to local state for that object. -->
Cookie对于将传递给另一个进程并在稍后返回的对象非常有用。 
通过使用其他进程无法访问的*scope*作为参数调用**zx_object_set_cookie()** 设置cookie值，稍后可以使用**zx_object_get_cookie()** 来验证句柄是否由调用进程“创建”并同时返回该对象的本地状态的ID或指针。

<!-- When the object referenced by *scope* is destroyed or if a handle to that object
is no longer available, the cookie may no longer be modified or obtained. -->
当*scope*引用的对象被销毁或者该对象的句柄不可用时，cookie值不可再被修改或获取。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_object_set_cookie**() returns **ZX_OK** on success.  In the event of failure,
a negative error value is returned. -->
**zx_object_set_cookie()** 执行成功则返回**ZX_OK**。
如果发生错误，将返回以下错误码之一。

<!-- ## ERRORS -->
## 错误码

<!-- 
**ZX_ERR_BAD_HANDLE**  *handle* or *scope* are not valid handles. -->
**ZX_ERR_BAD_HANDLE**：*handle*或*scope*是无效句柄。

<!-- **ZX_ERR_NOT_SUPPORTED**  *handle* is not a handle to an object that may have a cookie set. -->
**ZX_ERR_NOT_SUPPORTED**：*handle*不是可以具有cookie集合（即事件，事件对，资源或VMO）的对象句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  **zx_object_set_cookie**() was called previously with a different
object as the *scope*, or the cookie has not been set. -->
**ZX_ERR_ACCESS_DENIED**：先前已使用另一个对象作为*scope*调用了**zx_object_set_cookie()**，或者cookie值尚未设置。

<!-- ## SEE ALSO -->
## 另见

[object_get_cookie](object_get_cookie.md).
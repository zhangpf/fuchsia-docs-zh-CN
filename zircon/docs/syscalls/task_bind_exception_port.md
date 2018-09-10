# zx_task_bind_exception_port
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/af07ad38812f7566b6c859238ece1bb4c70b969e/docs/syscalls/task_bind_exception_port.md)

---
<!-- ## NAME -->
## 名称

<!-- task_bind_exception_port - Bind to, or unbind from, the exception port
corresponding to a given job, process, or thread. -->
task_bind_exception_port —— 在指定的作业，进程或线程上，绑定或取消绑定异常端口。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_task_bind_exception_port(zx_handle_t object, zx_handle_t eport, uint64_t key, uint32_t options);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **task_bind_exception_port**() is used to bind (or unbind) a port to
the exception port of a job, process, or thread. -->
**task_bind_exception_port()** 用于将端口绑定（或解除绑定）到作业，进程或线程的异常端口上。

<!-- *eport* is an IO port created by [zx_port_create](port_create.md). The same
IO port can be bound to multiple objects. -->
其中*eport*参数是由[zx_port_create](port_create.md)创建的IO端口。 
同一个IO端口可以绑定到多个对象上。

<!-- *key* is passed back in exception reports, and is part of the port
message protocol. -->
*key*在后续的异常报告中被传回，这也是端口消息协议的一部分。

<!-- When a port is bound to the exception port of an object it participates
in exception processing. See below for how exceptions are processed. -->
当端口绑定到对象的异常端口时，它将参与到异常的处理中。 
请参阅下文了解异常是如何处理的。

<!-- ### Unbinding -->
### 解除绑定

<!-- To unbind from an exception port pass **ZX_HANDLE_INVALID** for *eport*.
This will remove the exception port from *object* and *eport* will no
longer participate in exception processing for *object*. -->
为了从异常端口取消绑定，请传递**ZX_HANDLE_INVALID**到*eport*。 
这将从*object*中删除异常端口，并且*eport*将不再参与到*object*的异常处理中。

<!-- The exception port will unbind automatically if all handles to *eport*
are closed while it is still bound. -->
指向*eport*的所有句柄均已关闭时，如果*eport*仍处于绑定状态，那么这些异常端口将自动解除绑定。

<!-- A thread may be currently waiting for a response from the program that
bound *eport* when it is unbound. There are two choices for what happens
to the thread: -->
线程当前可能正在等待绑定*eport*的程序在解除绑定时的响应，此时对于线程可以有两种选择：

<!-- - Have exception processing continue as if *eport* had never been bound.
This is the default behavior. -->
- 继续进行异常处理，就好像*eport*从未被绑定一样，并且这也是默认行为。
<!-- - Have the thread continue to wait for a response from the same kind
of exception port as *eport*. This is done by passing
*ZX_EXCEPTION_PORT_UNBIND_QUIETLY* in *options* when unbinding *eport*.
This option is useful, for example, when a debugger wants to detach from the
thread's process, but leave the thread in stasis waiting for an exception
response. -->

- 让线程继续等待与*eport*相同类型的异常端口的响应。 
  这是通过在取消绑定*eport*时传递*ZX_EXCEPTION_PORT_UNBIND_QUIETLY*到*options*参数来实现的。 
  例如，当调试器想要从线程的进程中分离，但又想让线程处于停滞状态等待异常响应时，此选项便很有用。
<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **task_bind_exception_port**() returns **ZX_OK** on success.
In the event of failure, a negative error value is returned. -->
**task_bind_exception_port()** 调用成功则返回*ZX_OK*。
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_ALREADY_BOUND** *object* already has its exception port bound. -->

**ZX_ERR_ALREADY_BOUND**：*object*已经绑定了异常端口。

<!-- 
**ZX_ERR_BAD_HANDLE** *object* is not a valid handle,
or *eport* is not a valid handle. Note that when unbinding from an exception
port *eport* is **ZX_HANDLE_INVALID**. -->
**ZX_ERR_BAD_HANDLE**： *object*是无效句柄，或*eport*是无效句柄。 
请注意，当从异常端口解除绑定时，参数*eport*的值是**ZX_HANDLE_INVALID**。

<!-- **ZX_ERR_WRONG_TYPE**  *object* is not that of a job, process, or thread,
and is not **ZX_HANDLE_INVALID**,
or *eport* is not that of a port and is not **ZX_HANDLE_INVALID**. -->
**ZX_ERR_WRONG_TYPE**：*object*不是作业，进程或线程类型的对象且不是**ZX_HANDLE_INVALID**，或*eport*不是端口类型对象且不是**ZX_HANDLE_INVALID**。

<!-- **ZX_ERR_INVALID_ARGS** A bad value has been passed in *options*. -->
**ZX_ERR_INVALID_ARGS**：向*options*参数中传递了错误的值。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见
[exceptions](../exceptions.md).
[port_create](port_create.md).
[port_wait](port_wait.md).
[task_resume](task_resume.md).
# zx_socket_accept
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/socket_accept.md)

---

<!-- socket_accept - receive another socket object via a socket -->
socket_accept —— 通过socket接收另一个socket对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_socket_accept(zx_handle_t socket, zx_handle_t* out_socket);
```

<!-- ### DESCRIPTION -->
### 描述

<!-- **socket_accept**() attempts to receive a new socket via an existing socket
connection.  The signal **ZX_SOCKET_ACCEPT** is asserted when there is a new
socket available. -->
**socket_accept()** 尝试通过现有的socket连接接收新socket对象。当有新socket可读时，信号**ZX_SOCKET_ACCEPT** 将会被置位。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **socket_accept**() returns **ZX_OK** on success and the received handle
is returned via *out_socket*.  In the event of failure, one of the following
values is returned. -->
**socket_accept()** 成功则返回**ZX_OK**，并通过*out_socket*返回接收的socket句柄。如果调用失败，则返回下列错误码之一。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  The handle *socket* is invalid. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  The handle *socket* is not a socket handle. -->
**ZX_ERR_WRONG_TYPE**：*socket*不是socket类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  The handle *socket* lacks **ZX_RIGHT_READ**. -->
**ZX_ERR_ACCESS_DENIED**：*socket*句柄缺少**ZX_RIGHT_READ**权限。

<!-- **ZX_ERR_INVALID_ARGS**  *out_socket* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*out_socket*是无效指针。

<!-- **ZX_ERR_SHOULD_WAIT**  There is no new socket ready to be accepted. -->
**ZX_ERR_SHOULD_WAIT**：没有新socket可接收。

<!-- **ZX_ERR_NOT_SUPPORTED**  This socket does not support the transfer of sockets.
It was not created with the **ZX_SOCKET_HAS_ACCEPT** option. -->
**ZX_ERR_NOT_SUPPORTED**：此socket不支持传输其他socket，即该socket不是使用**ZX_SOCKET_HAS_ACCEPT**标志位创建的。

<!-- ## LIMITATIONS -->
## 限制

<!-- The socket accept queue is only one element deep. -->
socket接收队列深度仅为1。

<!-- ## SEE ALSO -->
## 另见

<!-- [socket_create](socket_create.md),
[socket_read](socket_read.md),
[socket_share](socket_share.md),
[socket_write](socket_write.md). -->

[socket_create](socket_create.md)，[socket_read](socket_read.md)，[socket_share](socket_share.md)，[socket_write](socket_write.md)。
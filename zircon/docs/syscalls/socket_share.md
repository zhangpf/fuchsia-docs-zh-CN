# zx_socket_share
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/socket_share.md)

---

<!-- socket_share - send another socket object via a socket -->
socket_share —— 通过socket发送另一个socket对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_socket_share(zx_handle_t socket, zx_handle_t socket_to_send);
```

<!-- ### DESCRIPTION -->
### 描述

<!-- **socket_share**() attempts to send a new socket via an existing socket
connection.  The signal **ZX_SOCKET_SHARE** is asserted when it is possible
to send a socket. -->
**socket_share()** 尝试通过现有socket连接发送新socket。当可发送socket时，信号**ZX_SOCKET_SHARE**被置位。

<!-- On success, the *socket_to_send* is placed into the *socket*'s share
queue, and is no longer accessible to the caller's process. On any
failure, *socket_to_send* is discarded rather than transferred. -->
发送成功时，*socket_to_send*被放入*socket*的共享队列中，并且调用者的进程不可再访问它。如果发生任何错误，*socket_to_send*将被丢弃（而非被传输）。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **socket_share**() returns **ZX_OK** on success.  In the event of failure,
one of the following values is returned. -->
**socket_share()** 执行成功则返回**ZX_OK**。如果发生错误，将返回以下值之一。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  The handle *socket* or *socket_to_send* is invalid. -->
**ZX_ERR_BAD_HANDLE**：*socket*或*socket_to_send*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  The handle *socket* or *socket_to_send* is not a socket handle. -->
**ZX_ERR_WRONG_TYPE**：*socket*或*socket_to_send*不是socket类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  The handle *socket* lacks **ZX_RIGHT_WRITE** or
the handle *socket_to_send* lacks **ZX_RIGHT_TRANSFER**. -->
**ZX_ERR_ACCESS_DENIED**：*socket*缺少**ZX_RIGHT_WRITE**权限，或*socket_to_send*缺少**ZX_RIGHT_TRANSFER**权限。

<!-- 
**ZX_ERR_BAD_STATE**  The *socket_to_send* was a handle to the same socket
as *socket* or to the other endpoint of *socket* or the *socket_to_send* itself
is capable of sharing. -->
**ZX_ERR_BAD_STATE**： 下列情况之一：*socket_to_send*与*socket*是相同的socket句柄；*socket_to_send*与*socket*的是相同的socket句柄；*socket_to_send*本身能够共享。

<!-- **ZX_ERR_SHOULD_WAIT**  There is already a socket in the share queue. -->
**ZX_ERR_SHOULD_WAIT**：共享队列中已有socket。

<!-- **ZX_ERR_NOT_SUPPORTED**  This socket does not support the transfer of sockets.
It was not created with the ZX_SOCKET_HAS_ACCEPT option. -->
**ZX_ERR_NOT_SUPPORTED**：此socket不支持传输其他socket，即它不是使用*ZX_SOCKET_HAS_ACCEPT*标志位创建的。

<!-- **ZX_ERR_PEER_CLOSED** The socket endpoint's peer is closed. -->
**ZX_ERR_PEER_CLOSED**：socket的相对端点已关闭。

<!-- ## LIMITATIONS -->
## 限制

<!-- The socket share queue is only element deep. -->
socket的共享队列深度仅为1。


<!-- ## SEE ALSO -->
## 另见

<!-- [socket_accept](socket_accept.md),
[socket_create](socket_create.md),
[socket_read](socket_read.md),
[socket_write](socket_write.md). -->

[socket_accept](socket_accept.md)，[socket_create](socket_create.md)，[socket_read](socket_read.md)，[socket_write](socket_write.md)。
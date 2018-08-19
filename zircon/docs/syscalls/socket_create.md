# zx_socket_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/socket_create.md)

---

<!-- ## NAME -->
## 名称

<!-- socket_create - create a socket -->
socket_create —— 创建socket对象

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_socket_create(uint32_t options,
                             zx_handle_t* out0, zx_handle_t* out1);

```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**socket_create**() creates a socket, a connected pair of
bidirectional stream transports, that can move only data, and that
have a maximum capacity. -->

**socket_create()** 创建socket对象，一对只能传输数据，并且具有最大容量限制的双向流式传输的连接。

<!-- Data written to one handle may be read from the opposite. -->
写入句柄的数据可以在相对端点的句柄上被读取。

<!-- The *options* must set either the **ZX_SOCKET_STREAM** or
**ZX_SOCKET_DATAGRAM** flag. -->
*options*必须设置**ZX_SOCKET_STREAM**或**ZX_SOCKET_DATAGRAM**标志位。


<!-- The **ZX_SOCKET_HAS_CONTROL** flag may be set to enable the
socket control plane. -->

可以设置**ZX_SOCKET_HAS_CONTROL**标志位，以启用socket控制面传输。

<!-- 
The **ZX_SOCKET_HAS_ACCEPT** flag may be set to enable transfer
of sockets over this socket via **socket_share**() and **socket_accept**(). -->

可以设置**ZX_SOCKET_HAS_ACCEPT**标志位，以启用通过**socket_share()** 和**socket_accept()** 在此socket上传输其它socket对象。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **socket_create**() returns **ZX_OK** on success. In the event of
failure, one of the following values is returned. -->
**socket_create()** 执行成功则返回**ZX_OK**。如果发生错误，将返回以下值之一。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *out0* or *out1* is an invalid pointer or NULL or
*options* is any value other than **ZX_SOCKET_STREAM** or **ZX_SOCKET_DATAGRAM**. -->
**ZX_ERR_INVALID_ARGS**： *out0*或*out1*是无效指针或NULL，或*options*是除*ZX_SOCKET_STREAM*或*ZX_SOCKET_DATAGRAM*之外的任何值。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## LIMITATIONS -->
## 限制

<!-- The maximum capacity is not currently set-able. -->
目前无法设置最大容量。

<!-- ## SEE ALSO -->
## 另见

<!-- [socket_accept](socket_accept.md),
[socket_read](socket_read.md),
[socket_share](socket_share.md),
[socket_write](socket_write.md). -->

[socket_accept](socket_accept.md)，[socket_read](socket_read.md)，[socket_share](socket_share.md)，[socket_write](socket_write.md)。
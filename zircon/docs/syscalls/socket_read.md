# zx_socket_read
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/socket_read.md)

---

<!-- ## NAME -->
## 名称

<!-- socket_read - read data from a socket -->
socket_read —— 从socket中读取数据

<!-- ## SYNOPSIS -->
## 概要
```
#include <zircon/syscalls.h>

zx_status_t zx_socket_read(zx_handle_t handle, uint32_t options,
                           void* buffer, size_t buffer_size,
                           size_t* actual) {
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **socket_read**() attempts to read *buffer_size* bytes into *buffer*. If
successful, the number of bytes actually read are return via
*actual*. -->
**socket_read()** 尝试读取*buffer_size*个字节数据到*buffer*中。如果读取成功，则实际读取的字节数将通过*actual*返回。

<!-- If a NULL *buffer* and 0 *buffer_size* are passed in, then this syscall
instead requests that the number of outstanding bytes to be returned
via *actual*. -->

如果传入*buffer*的是NULL，并且*buffer_size*为0，则此系统调用会请求未完成字节数，并通过*actual*返回。

<!-- If a NULL *actual* is passed in, it will be ignored. -->
如果传入*actual*的值为NULL，那么它将会被忽略。

<!-- If the socket was created with **ZX_SOCKET_DATAGRAM**, this syscall reads
only the first available datagram in the socket (if one is present).
If *buffer* is too small for the datagram, then the read will be
truncated, and any remaining bytes in the datagram will be discarded. -->
如果socket是使用*ZX_SOCKET_DATAGRAM*标志位创建的，则此系统调用仅读取socket中的第一个可用数据报（如果存在的话）。如果缓冲区对于数据报来说太小，则读取将被截断，并且数据报中的任何剩余字节将被丢弃。

<!-- If *options* is set to **ZX_SOCKET_CONTROL**, then **socket_read**()
attempts to read from the socket control plane. -->
如果*options*设置了**ZX_SOCKET_CONTROL**标志位，则**socket_read()** 将尝试从socket控制面中读取。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **socket_read**() returns **ZX_OK** on success, and writes into
*actual* (if non-NULL) the exact number of bytes read. -->
**socket_read()** 执行成功则返回**ZX_OK**，并将准确的读取字节数写入*actual*（如果它非NULL）中。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_BAD_STATE** *options* includes **ZX_SOCKET_CONTROL** and the
socket was not created with **ZX_SOCKET_HAS_CONTROL**. -->
**ZX_ERR_BAD_STATE**：*options*包含**ZX_SOCKET_CONTROL**标志位，但该socket不是由**ZX_SOCKET_HAS_CONTROL**创建的。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a socket handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是*socket*类型句柄。

<!-- **ZX_ERR_INVALID_ARGS** If any of *buffer* or *actual* are non-NULL
but invalid pointers, or if *buffer* is NULL but *size* is positive,
or if *options* is not either zero or **ZX_SOCKET_CONTROL*. -->
**ZX_ERR_INVALID_ARGS**：*buffer*或*actual*中的任何一个是非NULL但无效指针；或*buffer*为NULL但*size*为正；或*options*不是0或*ZX_SOCKET_CONTROL*。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_READ**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*不具有**ZX_RIGHT_READ**权限。

<!-- **ZX_ERR_SHOULD_WAIT**  The socket contained no data to read. -->
**ZX_ERR_SHOULD_WAIT**：socket没有数据可读。

<!-- **ZX_ERR_PEER_CLOSED**  The other side of the socket is closed and no data is
readable. -->
**ZX_ERR_PEER_CLOSED**：socket的相对端点已关闭，并且没有数据可读。

<!-- **ZX_ERR_BAD_STATE**  Reading has been disabled for this socket endpoint. -->
**ZX_ERR_BAD_STATE**：已禁用通过此socket端点读取数据。

<!-- ## SEE ALSO -->
## 另见

<!-- [socket_create](socket_create.md),
[socket_write](socket_write.md). -->

[socket_create](socket_create.md)，[socket_write](socket_write.md)。
# Socket
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/objects/socket.md)

---
<!-- ## NAME -->
## 名称

<!-- Socket - Bidirectional streaming IPC transport -->
Socket —— 双向的流式IPC传输

<!-- ## SYNOPSIS -->
## 概要
<!-- 
Sockets are a bidirectional stream transport. Unlike channels, sockets
only move data (not handles). -->
socket是双向的流式传输。与channel的不同之处在于，socket仅能传输数据（而不移动句柄）。

<!-- ## DESCRIPTION -->
## 描述

<!-- Data is written into one end of a socket via *zx_socket_write* and
read from the opposing end via *zx_socket_read*. -->
通过*zx_socket_write*写入数据到socket的一端，并通过*zx_socket_read*从相对端点读取。

<!-- Upon creation, both ends of the socket are writable and readable. Via the
**ZX_SOCKET_SHUTDOWN_READ** and **ZX_SOCKET_SHUTDOWN_WRITE** options to
*zx_socket_write*, one end of the socket can be closed for reading and/or
writing. -->
创建时，socket的两端都是可写和可读的。 通过zx_socket_write的ZX_SOCKET_SHUTDOWN_READ和ZX_SOCKET_SHUTDOWN_WRITE选项，可以关闭socket的一端以禁用从这一端读取和/或写入数据。

<!-- ## PROPERTIES -->
## 属性
<!-- 
The following properties may be queried from a socket object: -->
可从socket对象查询以下属性：

<!-- **ZX_PROP_SOCKET_RX_BUF_MAX** maximum size of the receive buffer of a socket, in
bytes. The receive buffer may become full at a capacity less than the maximum
due to overheads. -->
**ZX_PROP_SOCKET_RX_BUF_MAX**：socket接收缓冲区的最大大小，以字节为单位。由于额外的开销，接收缓冲区可能在小于最大容量时候就已经变满。

<!-- **ZX_PROP_SOCKET_RX_BUF_SIZE** size of the receive buffer of a socket, in bytes. -->
**ZX_PROP_SOCKET_RX_BUF_SIZE**：socket接收缓冲区的大小，以字节为单位。

<!-- **ZX_PROP_SOCKET_TX_BUF_MAX** maximum size of the transmit buffer of a socket,
in bytes. The transmit buffer may become full at a capacity less than the
maximum due to overheads. -->
**ZX_PROP_SOCKET_TX_BUF_MAX**：socket发送缓冲区的最大大小，以字节为单位。由于额外的开销，发送缓冲区可能在小于最大容量时候就已经变满。

<!-- **ZX_PROP_SOCKET_TX_BUF_SIZE** size of the transmit buffer of a socket, in
bytes. -->
**ZX_PROP_SOCKET_TX_BUF_SIZE**：socket发送缓冲区的大小，以字节为单位。

<!-- From the point of view of a socket handle, the receive buffer contains the data
that is readable via **zx_socket_read**() from that handle (having been written
from the opposing handle), and the transmit buffer contains the data that is
written via **zx_socket_write**() to that handle (and readable from the opposing
handle). -->
从socket句柄的角度来看，接收缓冲区包含可通过**zx_socket_read()** 读取（从相对端点的句柄写入的）数据，同时发送缓冲区包含可通过**zx_socket_write()** 写入（可从相对端点的句柄读取）的数据。

<!-- ## SIGNALS -->
## 信号

<!-- The following signals may be set for a socket object: -->
下列信号可以被设置于socket对象之上：

<!-- **ZX_SOCKET_READABLE** data is available to read from the socket -->
**ZX_SOCKET_READABLE**：可从socket中读取数据

<!-- **ZX_SOCKET_WRITABLE** data may be written to the socket -->
**ZX_SOCKET_WRITABLE**：可将数据写入socket

<!-- **ZX_SOCKET_PEER_CLOSED** the other endpoint of this socket has
been closed. -->
**ZX_SOCKET_PEER_CLOSED**：socket的相对端点已关闭。

<!-- **ZX_SOCKET_READ_DISABLED** reading (beyond already buffered data) is disabled
permanently for this endpoint either because of passing
**ZX_SOCKET_SHUTDOWN_READ** to this endpoint or passing
**ZX_SOCKET_SHUTDOWN_WRITE** to the peer. Reads on a socket endpoint with this
signal raised will succeed so long as there is data in the socket that was
written before reading was disabled. -->
**ZX_SOCKET_SHUTDOWN_READ**：由于传递**ZX_SOCKET_READ_DISABLED** 到此端点或将**ZX_SOCKET_SHUTDOWN_WRITE** 传递到相对端点，此端点被永久禁用读取（除了已缓冲的数据）。但在禁用读取之前写入socket中的数据，读取socket依然会返回成功。

<!-- **ZX_SOCKET_WRITE_DISABLED** writing is disabled permanently for this endpoint
either because of passing **ZX_SOCKET_SHUTDOWN_WRITE** to this endpoint or
passing **ZX_SOCKET_SHUTDOWN_READ** to the peer. -->
**ZX_SOCKET_WRITE_DISABLED**：因为将**ZX_SOCKET_SHUTDOWN_WRITE**传递给此端点或将**ZX_SOCKET_SHUTDOWN_READ**传递给相对端点，此端点永久禁用数据写入。

<!-- **ZX_SOCKET_CONTROL_READABLE** data is available to read from the
socket control plane. -->

**ZX_SOCKET_CONTROL_READABLE**：可从socket控制面读取数据。


<!-- **ZX_SOCKET_CONTROL_WRITABLE** data may be written to the socket control plane. -->
**ZX_SOCKET_CONTROL_WRITABLE**：可将数据写入到socket控制面中。

<!-- **ZX_SOCKET_SHARE** a socket may be sent via *zx_socket_share*. -->
**ZX_SOCKET_SHARE**：可以通过*zx_socket_share*发送socket。

<!-- **ZX_SOCKET_ACCEPT** a socket may be received via *zx_socket_accept*. -->
**ZX_SOCKET_ACCEPT**：可以通过*zx_socket_accept*接收socket。

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [socket_accept](../syscalls/socket_accept.md) - receive a socket via a socket
+ [socket_create](../syscalls/socket_create.md) - create a new socket
+ [socket_read](../syscalls/socket_read.md) - read data from a socket
+ [socket_share](../syscalls/socket_share.md) - share a socket via a socket
+ [socket_write](../syscalls/socket_write.md) - write data to a socket -->

+ [socket_accept](../syscalls/socket_accept.md) —— 通过socket接收另一个socket对象
+ [socket_create](../syscalls/socket_create.md) —— 创建新socket
+ [socket_read](../syscalls/socket_read.md) —— 从socket中读取数据
+ [socket_share](../syscalls/socket_share.md) —— 通过socket发送另一个socket对象
+ [socket_write](../syscalls/socket_write.md) —— 写入数据到socket
# zx_socket_write
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/socket_write.md)

---

<!-- ## NAME -->
## 名称

<!-- socket_write - write data to a socket -->
socket_write —— 写入数据到socket

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_socket_write(zx_handle_t handle, uint32_t options,
                            const void* buffer, size_t buffer_size,
                            size_t* actual) {
```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**socket_write**() attempts to write *buffer_size* bytes to the socket specified
by *handle*. The pointer to *bytes* may be NULL if *buffer_size* is zero. -->
**socket_write()** 尝试将*buffer_size*字节的数据写入*handle*指定的socket中。如果*buffer_size*为0，则指向*bytes*的指针可为NULL。

<!-- If *buffer_size* is zero, a bitwise combination of **ZX_SOCKET_SHUTDOWN_READ** and
**ZX_SOCKET_SHUTDOWN_WRITE** can be passed to *options* to disable reading or
writing from a socket endpoint: -->
如果*buffer_size*为0，可将**ZX_SOCKET_SHUTDOWN_READ**和**ZX_SOCKET_SHUTDOWN_WRITE** 标志位的按位组合值传递给*options*以禁用从socket端点读取或写入：
<!-- 
 * If **ZX_SOCKET_SHUTDOWN_READ** is passed to *options*, and *buffer_size* is
   0, then reading is disabled for the socket endpoint at *handle*. All data
   buffered in the socket at the time of the call can be read, but further reads
   from this endpoint or writes to the other endpoint of the socket will fail
   with **ZX_ERR_BAD_STATE**. -->
  * 如果将**ZX_SOCKET_SHUTDOWN_READ**标志位传递给*options*，并且*buffer_size*为0，则禁用在*handle*处的socket端点的数据读取。此时，还可以继续读取已在socket中缓冲的所有数据，但是未来从该端点读取或从相对端点写入都将失败并返回**ZX_ERR_BAD_STATE**错误码。
<!-- 
 * If **ZX_SOCKET_SHUTDOWN_WRITE** is passed to *options*, and *buffer_size* is
   0, then writing is disabled for the socket endpoint at *handle*. Further
   writes to this endpoint or reads from the other endpoint of the socket will
   fail with **ZX_ERR_BAD_STATE**. -->
  * 如果将**ZX_SOCKET_SHUTDOWN_WRITE**传递给*options*，并且*buffer_size*为0，则禁用在*handle*处的socket端点的数据写入。未来从此端点的写入或从相对端点读取都将失败并返回**ZX_ERR_BAD_STATE**错误码。
  
<!-- If **ZX_SOCKET_CONTROL** is passed to *options*, then **socket_write**()
attempts to write into the socket control plane. A write to the control plane is
never short. If the socket control plane has insufficient space for *buffer*, it
writes nothing and returns **ZX_ERR_OUT_OF_RANGE**. -->
如果将**ZX_SOCKET_CONTROL**传递给*options*，则**socket_write()** 会尝试写入socket控制面。写入控制面的数据不能部分缺失，所以如果socket控制面没有足够的空间用于存放*buffer*，则它不会写入任何内容并返回**ZX_ERR_OUT_OF_RANGE**。

<!-- If a NULL *actual* is passed in, it will be ignored. -->
如果传入*actual*是NULL，那么它会被忽略。


<!-- A **ZX_SOCKET_STREAM** socket write can be short if the socket does not have
enough space for all of *buffer*. If a non-zero amount of data was written to
the socket, the amount written is returned via *actual* and the call succeeds.
Otherwise, if the socket was already full, the call returns
**ZX_ERR_SHOULD_WAIT** and the client should wait (e.g., with
[object_wait_one](object_wait_one.md) or
[object_wait_async](object_wait_async.md)). -->
如果socket没有足够的空间用于存放*buffer*中的所有数据，则**ZX_SOCKET_STREAM**socket写入的数据可能会部分缺失。如果向socket写入非零数据量，则通过*actual*返回写入的数量，并且调用成功。否则，如果socket已满，则调用返回**ZX_ERR_SHOULD_WAIT**，并且客户端应该等待（例如，使用[object_wait_one](object_wait_one.md)或
[object_wait_async](object_wait_async.md)）。

<!-- A **ZX_SOCKET_DATAGRAM** socket write is never short. If the socket has
insufficient space for *buffer*, it writes nothing and returns
**ZX_ERR_SHOULD_WAIT**. If the write succeeds, *buffer_size* is returned via
*actual*. -->
**ZX_SOCKET_DATAGRAM**类型的socket写入不能部分缺失，所以如果socket没有足够的空间用于存放*buffer*，则它不会写入任何内容并返回**ZX_ERR_SHOULD_WAIT**。如果写入成功，则通过*actual*返回*buffer_size*。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **socket_write**() returns **ZX_OK** on success. -->
**socket_write()** 执行成功时返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。


<!-- **ZX_ERR_BAD_STATE**  *options* includes **ZX_SOCKET_CONTROL** and the
socket was not created with **ZX_SOCKET_HAS_CONTROL**. -->
**ZX_ERR_BAD_STATE**：*options*包含**ZX_SOCKET_CONTROL**标志位，但该socket不是由**ZX_SOCKET_HAS_CONTROL**创建的。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a socket handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是socket类型句柄。

<!-- 
**ZX_ERR_INVALID_ARGS**  *buffer* is an invalid pointer,
**ZX_SOCKET_SHUTDOWN_READ** and/or **ZX_SOCKET_SHUTDOWN_WRITE** was passed to
*options* but *buffer_size* was not 0, or *options* was not 0 or a combination
or **ZX_SOCKET_SHUTDOWN_READ** and/or **ZX_SOCKET_SHUTDOWN_WRITE**. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一：*buffer*是无效指针；*options*包含**ZX_SOCKET_SHUTDOWN_READ**和/或**ZX_SOCKET_SHUTDOWN_WRITE**标志位但*buffer_size*不为0；*options*不是0或**ZX_SOCKET_SHUTDOWN_READ** 和/或 **ZX_SOCKET_SHUTDOWN_WRITE**的组合。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_WRITE**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*不具有**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_SHOULD_WAIT**  The buffer underlying the socket is full, or
the socket was created with **ZX_SOCKET_DATAGRAM** and *buffer* is
larger than the remaining space in the socket. -->
**ZX_ERR_SHOULD_WAIT**: socket底层的缓冲区已满，或socket
是使用**ZX_SOCKET_DATAGRAM**标志位创建的并且*buffer*大于socket中的剩余空间。

<!-- **ZX_ERR_BAD_STATE**  Writing has been disabled for this socket endpoint. -->
**ZX_ERR_BAD_STATE**：已禁用通过此socket端点写入数据。

<!-- **ZX_ERR_PEER_CLOSED**  The other side of the socket is closed. -->
**ZX_ERR_PEER_CLOSED**：socket的相对端点已关闭。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

<!-- [socket_create](socket_create.md),
[socket_read](socket_read.md). -->
[socket_create](socket_create.md)，[socket_read](socket_read.md)。
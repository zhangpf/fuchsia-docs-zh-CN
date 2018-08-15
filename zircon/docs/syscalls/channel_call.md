# zx_channel_call
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/channel_call.md)

---
<!-- ## NAME -->
## 名称

<!-- channel_call - send a message to a channel and await a reply -->
channel_call —— 向channel发送消息并等待响应

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

typedef struct {
    void* wr_bytes;
    zx_handle_t* wr_handles;
    void *rd_bytes;
    zx_handle_t* rd_handles;
    uint32_t wr_num_bytes;
    uint32_t wr_num_handles;
    uint32_t rd_num_bytes;
    uint32_t rd_num_handles;
} zx_channel_call_args_t;

zx_status_t zx_channel_call(zx_handle_t handle, uint32_t options,
                            zx_time_t deadline, zx_channel_call_args_t* args,
                            uint32_t* actual_bytes, uint32_t* actual_handles);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **channel_call**() is like a combined **channel_write**(), **object_wait_one**(),
and **channel_read**(), with the addition of a feature where a transaction id at
the front of the message payload *bytes* is used to match reply messages with send
messages, enabling multiple calling threads to share a channel without any additional
userspace bookkeeping. -->
**channel_call()** 类似于**channel_write()**，**object_wait_one()** 和**channel_read()** 三者的组合。只是在此基础上添加了一个功能——消息有效负载*字节*前面的事务id用于匹配回复消息和发送消息，并启用多个调用线程来共享同一channel，而无需任何额外的用户空间记录。

<!-- The write and read phases of this operation behave like **channel_write**() and
**channel_read**() with the difference that their parameters are provided via the
*zx_channel_call_args_t* structure. -->
此操作的在写入和读取阶段的行为类似于**channel_write()** 和**channel_read()**，区别在于它们的参数是通过*zx_channel_call_args_t*结构体提供的。

<!-- The first four bytes of the written and read back messages are treated as a
transaction ID of type **zx_txid_t**.  The kernel generates a txid for the
written message, replacing that part of the message as read from userspace.
The kernel generated txid will be between 0x80000000 and 0xFFFFFFFF, and will
not collide with any txid from any other **channel_call**() in progress against
this channel endpoint.  If the written message has a length of fewer than four
bytes, an error is reported. -->
写入和回读消息的前四个字节被当作类型是**zx_txid_t** 的事务ID。内核为写入的消息生成txid，并将消息的那部分替换为从用户空间读取的消息。内核生成的txid值处于0x80000000和0xFFFFFFFF之间，并且不会与来自此channel端点的任何其他**channel_call()** 的任何txid发生冲突。如果写入的消息的长度少于四个字节，则会报告错误。

<!-- When the outbound message is written, simultaneously an interest is registered
for inbound messages of the matching txid. -->
当写入传出消息的同时，同一txid的传入消息的兴趣会被同时注册上。

<!-- 
While *deadline* has not passed, if an inbound message arrives with a matching txid,
instead of being added to the tail of the general inbound message queue, it is delivered
directly to the thread waiting in **zx_channel_call**(). -->
在*deadline*尚未到来时，如果传入消息在到达时txid匹配，则不会添加到常规传入消息队列的尾部，而是直接将其传递给在**zx_channel_call()** 中等待的线程。

<!-- If such a reply arrives after *deadline* has passed, it will arrive in the general
inbound message queue, cause **ZX_CHANNEL_READABLE** to be signaled, etc. -->
如果在*deadline*之后回复到达，它将到达常规的传入消息队列，并导致**ZX_CHANNEL_READABLE**信号被触发，等等。

<!-- Inbound messages that are too large to fit in *rd_num_bytes* and *rd_num_handles*
are discarded and **ZX_ERR_BUFFER_TOO_SMALL** is returned in that case. -->
在这种情况下，将丢弃太大而不适合*rd_num_bytes*和*rd_num_handles*的传入消息，并返回**ZX_ERR_BUFFER_TOO_SMALL**。

<!-- As with **zx_channel_write**(), the handles in *handles* are always consumed by
**zx_channel_call**() and no longer exist in the calling process. -->
与**zx_channel_write()** 一样，*handles*中的句柄始终由**zx_channel_call()** 持有，并不再存在于调用进程中。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **channel_call**() returns **ZX_OK** on success and the number of bytes and
count of handles in the reply message are returned via *actual_bytes* and
*actual_handles*, respectively. -->
*channel_call()* 调用成功则返回**ZX_OK**，并分别通过*actual_bytes*和*actual_handles*返回消息中的字节数和句柄数。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle, any element in
*handles* is not a valid handle, or there are duplicates among the handles
in the *handles* array. -->
**ZX_ERR_BAD_HANDLE**：*handle*不是有效的句柄，*handles*中存在不是有效句柄的值，或*handles*数组中的句柄存在重复项。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a channel handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是channel句柄。

<!-- **ZX_ERR_INVALID_ARGS**  any of the provided pointers are invalid or null,
or *wr_num_bytes* is less than four, or *options* is nonzero. -->
**ZX_ERR_INVALID_ARGS**：提供的指针存在无效或为null的，或*wr_num_bytes*小于4，或者*options*为非零。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have **ZX_RIGHT_WRITE** or
any element in *handles* does not have **ZX_RIGHT_TRANSFER**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_WRITE**权限，或*handles*中的存在没有**ZX_RIGHT_TRANSFER**权限的元素。

<!-- **ZX_ERR_PEER_CLOSED**  The other side of the channel was closed or became
closed while waiting for the reply. -->
**ZX_ERR_PEER_CLOSED**：在等待回复时，channel的另一侧已关闭或将要被关闭。

<!-- **ZX_ERR_CANCELED**  *handle* was closed while waiting for a reply. -->
**ZX_ERR_CANCELED**：*handle*在等待回复时被关闭。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_OUT_OF_RANGE**  *wr_num_bytes* or *wr_num_handles* are larger than the
largest allowable size for channel messages. -->
**ZX_ERR_OUT_OF_RANGE**： *wr_num_bytes*或*wr_num_handles*大于channel最大允许的消息大小。

<!-- **ZX_ERR_BUFFER_TOO_SMALL**  *rd_num_bytes* or *rd_num_handles* are too small
to contain the reply message. -->
**ZX_ERR_BUFFER_TOO_SMALL**：*rd_num_bytes*或*rd_num_handles*太小而无法存放回复消息。
<!-- 
**ZX_ERR_NOT_SUPPORTED**  one of the handles in *handles* was *handle*
(the handle to the channel being written to). -->
**ZX_ERR_NOT_SUPPORTED**: *handles*中的一个句柄是（正在被写入的通道句柄的）*handle*本身。

<!-- ## NOTES -->
## 注意

<!-- The facilities provied by **channel_call**() can interoperate with message dispatchers
using **channel_read**() and **channel_write**() directly, provided the following rules
are observed: -->

如果遵循以下规则，**channel_call()** 提供的工具可以直接与使用**channel_read()** 和**channel_write()** 的消息调度程序进行互操作：

<!-- 1. A server receiving synchronous messages via **channel_read**() should ensure that the
txid of incoming messages is reflected back in outgoing responses via **channel_write**()
so that clients using **channel_call**() can correctly route the replies. -->
1. 通过**channel_read()** 接收同步消息的服务端应确保传入消息的txid通过**channel_write()** 反映在传出响应中，以便使用**channel_call()** 的客户端可以正确路由到回复消息。

<!-- 2. A client sending messages via **channel_write**() that will be replied to should ensure
that it uses txids between 0 and 0x7FFFFFFF only, to avoid colliding with other threads
communicating via **channel_call**(). -->
2. 通过**channel_write()** 发送消息的客户端确保它仅使用介于0和0x7FFFFFFF之间的txid，以避免与通过**channel_call()** 通信的其他线程发生冲突。

<!-- If a **channel_call**() returns due to **ZX_ERR_TIMED_OUT**, if the server eventually replies,
at some point in the future, the reply *could* match another outbound request (provided about
2^31 **channel_call**()s have happened since the original request.  This syscall is designed
around the expectation that timeouts are generally fatal and clients do not expect to continue
communications on a channel that is timing out. -->
如果**channel_call()** 由于**ZX_ERR_TIMED_OUT** 原因而返回，如果服务端在将来的某个时刻最终回复消息，则回复*可能*会匹配到另一个传出请求上（自原先求以来已经经历了大约2^31个**channel_call()**）。这我们的设计中，这是安全的，因为此系统调用的设计期望是，超时通常是致命错误导致的，所以客户端不太可能在超时的通道上继续通信。

<!-- ## SEE ALSO -->
## 另见

<!-- [handle_close](handle_close.md),
[handle_duplicate](handle_duplicate.md),
[handle_replace](handle_replace.md),
[object_wait_one](object_wait_one.md),
[object_wait_many](object_wait_many.md),
[channel_create](channel_create.md),
[channel_read](channel_read.md),
[channel_write](channel_write.md). -->

[handle_close](handle_close.md)

[handle_duplicate](handle_duplicate.md)

[handle_replace](handle_replace.md)

[object_wait_one](object_wait_one.md)

[object_wait_many](object_wait_many.md)

[channel_create](channel_create.md)

[channel_read](channel_read.md)
 
[channel_write](channel_write.md)
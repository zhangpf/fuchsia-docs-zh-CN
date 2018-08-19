# Channel
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/objects/channel.md)

---
<!-- ## NAME -->
## 名称

<!-- channel - Bidirectional interprocess communication -->
channel —— 进程间的双向通信

<!-- ## SYNOPSIS -->
## 概要

<!-- A channel is a bidirectional transport of messages consisting of some
amount of byte data and some number of handles. -->
channel是包含字节数据和句柄的消息的双向传输通道。

<!-- ## DESCRIPTION -->
## 描述

<!-- Channels maintain an ordered queue of messages to be delivered in
either direction. A message consists of some amount of data and some
number of handles. A call to *zx_channel_write()* enqueues one message,
and a call to *zx_channel_read()* dequeues one message (if any are
queued). A thread can block until messages are pending via
*zx_object_wait_one()* or other waiting mechanisms. -->

channel维护了一个有序的消息队列，以便在任一方向上传递由一些数据和句柄组成的消息。调用*zx_channel_write()* 会将一条消息加入队列，而调用*zx_channel_read()* 会使一条消息出列（如果队列中消息存在的话）。线程可以被阻塞并挂起，直到通过*zx_object_wait_one()* 或其他等待机制获得消息。

<!-- Alternatively, a call to *zx_channel_call()* enqueues a message in one
direction of the channel, waits for a corresponding response, and
dequeues the response message. In call mode, corresponding responses
are identified via the first 4 bytes of the message, called the
transaction ID. The kernel supplies distinct transaction IDs (always with the
high bit set) for messages written with *zx_channel_call()*. -->
另外，调用*zx_channel_call()* 会在channel的一个方向上写入消息，并等待另一端的响应，而后从channel中读取响应消息。在call模式中，通过消息的前4个字节可以识别相应的响应，它被称为事务ID。内核为使用 *zx_channel_call()* 写入的消息提供不同的事务ID（始终将高位bit设置为1）。

<!-- The process of sending a message via a channel has two steps. The
first is to atomically write the data into the channel and move
ownership of all handles in the message into this channel. This
operation always consumes the handles: at the end of the call, all
handles either are all in the channel or are all discarded. The second operation
is similar: after a channel read, all the handles in the next message to read
are either atomically moved into the process's handle table, all remain in the
channel, or are discarded (only when the
**ZX_CHANNEL_READ_MAY_DISCARD** option is given). -->

通过channel发送消息的过程包括两个步骤。其一，将数据原子地写入channel，并将消息中所有句柄的所有权移动到此channel中。该操作将始终持有句柄：在调用结束时，所有句柄都已在channel中或全部被丢弃（在发生错误的情况下）。其二，和第一个操作类似：在读取通道之后，下一个要读取的消息中的所有句柄将原子地移动到进程的句柄表中，所以要么保留在channel中，要么被丢弃（仅当调用者指定*ZX_CHANNEL_READ_MAY_DISCARD*选项时）。

<!-- Unlike many other kernel object types, channels are not
duplicatable. Thus there is only ever one handle associated to a
handle endpoint. -->
与许多其他内核对象类型不同的是，channel不可复制。因此，只有一个与channel端点(endpoint)相关联的句柄。

<!-- Because of these properties (that channel messages move their handle
contents atomically, and that channels are not duplicatable), the
kernel is able to avoid complicated garbage collection, lifetime
management, or cycle detection simply by enforcing the simple rule
that a channel handle may not be written into itself. -->
由于这些属性（channel的消息以原子方式移动其句柄内容，并且channel不可复制），内核只需强制要求channel和handle不可被写入到自己处的简单规则，就可以避免复杂的垃圾回收，生命周期管理或循环检测等操作。

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [channel_call](../syscalls/channel_call.md) - synchronously send a message and receive a reply
+ [channel_create](../syscalls/channel_create.md) - create a new channel
+ [channel_read](../syscalls/channel_read.md) - receive a message from a channel
+ [channel_write](../syscalls/channel_write.md) - write a message to a channel -->
+ [channel_call](../syscalls/channel_call.md) —— 以同步的方式发送消息并返回响应
+ [channel_create](../syscalls/channel_create.md) —— 创建新的channel
+ [channel_read](../syscalls/channel_read.md) —— 从channel中读取消息
+ [channel_write](../syscalls/channel_write.md) —— 向channel写入消息

<br>

<!-- + [object_wait_one](../syscalls/object_wait_one.md) - wait for signals on one object -->
+ [object_wait_one](../syscalls/object_wait_one.md) —— 等待某个对象上的信号

<!-- ## SEE ALSO -->
## 另见

<!-- + [Zircon concepts](../concepts.md) -->
+ [Zircon中的概念](../concepts.md)
+ [Handles（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/handles.md)

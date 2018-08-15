<!-- # Life of an 'Open' -->
# `Open`的生命周期
---
[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/db14cb359edd7b30ca690bb36f86bff06046fbc5/the-book/life_of_an_open.md)

---
<!-- To provide an end-to-end picture of filesystem access on Fuchsia, this document
dives into the details of each layer which is used when doing something as
simple as opening a file. It’s important to note: all of these layers exist in
userspace; even when interacting with filesystem servers and drivers, the kernel
is merely used to pass messages from one component to another. -->
为了提供在Fuchsia上文件系统访问的端到端图景，本文档详细介绍了在执行诸如打开文件之类的简单操作时使用的每个层的详细信息。需要注意的是：所有这些层都存在于用户空间中；即使在与文件系统服务器和驱动程序进行跨进程交互时，内核也仅用于将消息从一个组件传递到另一个组件。

<!-- A call is made to:

`open(“foobar”);`

Where does that request go? -->
一次如下的调用操作：

`open(“foobar”);`

请求将发向何方？

<!-- ## Standard Library: Where 'open' is defined -->
## 标准库：`open`在何处定义？

<!-- The ‘open’ call is a function, provided by a [standard library](libc.md). For
C/C++ programs, this will normally be declared in `unistd.h`, which has a
backing definition in
[libfdio](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/fdio/).
For Go programs, there is an equivalent (but distinct) implementation in the Go
standard library. For each language and runtime, developers may opt into their
own definition of “open”. -->

`open`调用是由[标准库](libc.md)提供的函数。对于C/C++程序而言，通常在`unistd.h`中声明，并在[libfdio](https://github.com/fuchsia-mirror/zircon/tree/master/system/ulib/fdio)中有定义支持。对于Go程序而言，Go标准库中有一个等价（但不同）的实现。对于每种语言和运行时，开发人员可以选择加入他们自己对"open"的定义。
<!-- 
On a monolithic kernel, `open` would be a lightweight shim around a system
call, where the kernel might handle path parsing, redirection, etc. In that
model, the kernel would need to mediate access to resources based on exterior
knowledge about the caller. The Zircon kernel, however, intentionally has no
such system call. Instead, clients access filesystems through **channels** --
when a process is initialized, it may be provided a handle representing the
**root (“/”) directory** and a handle representing the current working
directory **(CWD)**.  Alternatively, in more exotic runtimes, one or more of
these handles may not be provided. In this example, however, involving a request
to open “foobar”, a relative path was used, so the incoming call could be sent
over the path representing the current working directory. -->
在宏内核操作系统中，`open`是系统调用的一个轻量级封装（shim），内核可能会处理路径解析，重定向等操作。在该模型中，内核需要基于关于调用者的外部知识来调解对资源的访问。然而，Zircon内核在设计上就没有这样的系统调用，相反，客户通过**channel**来访问文件系统：当进程在初始化时，进程可以提供表示**根（"/"）目录**的句柄和表示当前工作目录（**CWD**）的句柄。或者，在更奇特的运行时中，可能不会提供这些句柄中的一个或多个。但在这个例子中，涉及打开“foobar”的请求，使用了一个相对路径，所以进来的调用可以通过表示当前工作目录的路径来进行发送。

<!-- The standard library is responsible for taking a handle (or multiple handles)
and making them appear like file descriptors. As a consequence, the “file
descriptor table” is a notion that exists within a client process (if a client
chooses to use a custom runtime, they can view their resources purely as
handles -- the “file descriptor” wrapping is optional). -->
标准库负责处理一个（或多个）句柄，并使它们看起来像文件描述符。因此，“文件描述符表（file descriptor table）”是客户端进程中存在的概念（如果客户端选择使用自定义运行时，则可以纯粹将其资源视为句柄。因此，“文件描述符”封装是可选的）。

<!-- This raises a question, however: given a file descriptor to files, sockets,
pipes, etc, what does the standard library do to make all these resources
appear functionally the same? How does that client know what messages to send
over these handles? -->
但是，这里产生了一个问题：对于给文件，套接字，管道等分配的文件描述符，标准库需要做什么才能使所有这些资源在功能上看起来是一样的？客户又如何知道通过这些句柄发送什么消息？

## Fdio

<!-- A library called
[**fdio**](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/fdio/)
is responsible for providing a unified interface to a variety of resources --
files, sockets, services, pipes, and more. This layer defines a group of
functions, such as **read, write, open, close, seek, etc** that may be used on
file descriptors backed by a variety of protocols. Each supported protocol is
responsible for providing client-side code to interpret the specifics of their
interaction. For example, **sockets** provide multiple handles to clients; one
acting for data flow, and one acting as a control plane. In contrast, **files**
typically use only a single channel for control and data (unless extra work has
been done to ask for a memory mapping).  Although both sockets and files might
receive a call to `open` or `write`, they will need to interpret those commands
differently.

For the purposes of this document, we’ll be focusing on the primary protocol
used by filesystem clients: RemoteIO. -->


名为[**fdio**](https://github.com/fuchsia-mirror/zircon/tree/master/system/ulib/fdio/)的库负责为各种资源（文件，套接字，服务，管道等等）提供统一的接口。这一层定义了一组函数，例如 **读取，写入，打开，关闭，搜索等等**。这些函数可用于由各种协议支持的文件描述符。每个支持的协议都负责提供客户端代码来解释其交互的细节。例如，**套接字**为客户端提供多个句柄；一个用于数据流，另一个用作控制平面。相比之下，**文件**通常仅使用单个channel进行控制和数据（除非进行额外的工作来请求进行内存映射）。尽管套接字和文件可能会接收到对`open`或`write`的调用，但它们需要以不同的方式解释这些命令。

为了本文的目的，我们将重点关注文件系统客户端使用的主要协议：RemoteIO。

## RemoteIO
<!-- 
A program calling `open("foo")` will have called into the standard library,
found an “fdio” object corresponding to the current working directory, and will
need to send a request to a remote server to “please open foo”. How can this be
accomplished? The program has the following tools: -->

调用`open（“foo”）`的程序将调用标准库，找到与当前工作目录对应的“fdio”对象，并且需要向远程服务器发送请求以“请打开foo”。这又是如何实现的？

该程序具有以下功能：
  <!-- * One or more **handles** representing a connection to the CWD
  * [zx_channel_write](https://fuchsia.googlesource.com/zircon/+/master/docs/syscalls/channel_write.md):
    A system call which can send bytes and handles (over a channel)
  * [zx_channel_read](https://fuchsia.googlesource.com/zircon/+/master/docs/syscalls/channel_read.md):
    A system call which can receive bytes and handles (over a channel)
  * [zx_object_wait_one](https://fuchsia.googlesource.com/zircon/+/master/docs/syscalls/object_wait_one.md):
    A system call which can wait for a handle to be readable / writable -->
   * 一个或多个**handle**表示与CWD的连接
   * [zx_channel_write](/zircon/docs/syscalls/channel_write.md)：发送字节和句柄（通过channel）的系统调用
   * [zx_channel_read](/zircon/docs/syscalls/channel_read.md)：接收字节和句柄（通过channel）的系统调用
   * [zx_object_wait_one（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/syscalls/object_wait_one.md)：等待句柄变成可读/可写状态的系统调用
<!-- 
Using these primitives, the client can write a message to the filesystem server
on the CWD handle, which the server can read and then respond to with a
“success” or “failure message” in a write back to the client. While the server
is crunching away, figuring out what to actually open, the client may or may
not choose to wait before trying to read the status message. -->

通过使用这些原语，客户端可以向CWD句柄上的文件系统服务器写入消息，服务端读取该消息，然后使用“成功”或“失败消息”来响应客户端。在服务端在处理并弄清楚实际打开什么的过程中时，在尝试读取状态消息之前，客户端可能会也可能不会选择等待。

<!-- It’s important that the client and server agree on the interpretation of those
N bytes and N handles when messages are transmitted or received: if there is
disagreement between them, messages might be dropped (or worse, contorted into
an unintended behavior). Additionally, if this protocol allowed the client to
have arbitrary control over the server, this communication layer would be ripe
for exploitation. -->

很重要的一点是：当消息被发送或接收时，客户端和服务端就这N字节和N个句柄的解释是达成一致的：如果它们之间不一致，则可能会被丢弃（或者更糟糕的是，消息被曲解并产生意想不到的行为）。另外，如果这个协议允许客户端对服务端有任意的控制权，该通信层就可能被漏洞利用。
<!-- 
The [RemoteIO protocol
(RIO)](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/fdio/include/lib/fdio/remoteio.h)
describes the wire-format of what these bytes and handles should actually mean
when transmitted between two entities. The protocol describes things like
“expected number of handles”, “enumerated operation”, and “data”. In our case,
`open("foo")` creates an `ZXRIO_OPEN` message, and sets the “data” field of the RIO
message to the string “foo”. Additionally, if any flags are passed to
open (such as `O_RDONLY, O_RDWR, O_CREAT`, etc) these flags would be placed in
the “arg” field of the rio structure. However, if the operation was changed
(to, for example, `write`), the interpretation of this message would be
altered. -->

[RemoteIO协议（RIO）](https://github.com/fuchsia-mirror/zircon/blob/master/system/ulib/fdio/include/lib/fdio/remoteio.h)描述了在两个实体之间传送时，这些编排格式的字节和句柄，实际上应该意味着什么。该协议描述了诸如“期望的句柄数”，“枚举操作”和“数据”之类的内容。在我们的例子中，`open（“foo”）`创建一个`ZXRIO_OPEN`消息，并将RIO消息的“data”字段设置为字符串“foo”。此外，如果任何标志被传递`open`操作（例如`O_RDONLY，O_RDWR，O_CREAT`等），这些标志将被放置在rio结构的“arg”字段中。但是，如果操作被改变（例如，`write`操作），则该消息的解释也将同时被改变。

<!-- Exact byte agreement at this layer is critical, as it allows communication
between drastically different runtimes: **processes which understand RIO can
communicate easily between C, C++, Go, Rust, Dart programs (and others)
transparently.** -->
这一层的精确字节协议是非常关键的，因为它允许在完全不同的运行时间之间进行通信：**理解RIO的进程可以在C，C ++，Go，Rust，Dart（和其他）程序之间很容易地进行透明通信。**

<!-- 
This protocol, although incredibly useful, has currently been
constructed by hand, and is only used by the lower-level components within the
system. There is an ongoing effort to re-specify Remote IO as a FIDL
interface, which is used by the higher-level components of the Fuchsia, to take
advantage of the multiple-runtime interoperability that FIDL provides. In the
future, the two protocols will become unified, and FIDL binding will be
automatically generated and typechecked in a variety of languages. -->
该协议虽然非常有用，但目前还必须手工进行构建，并且只能由系统内的低层次组件使用。为了利用FIDL提供的多运行时的互操作性，我们正在不断努力将Remote IO重新指定为可以被Fuchsia高级组件使用的FIDL接口。在将来，这两个协议将变得统一，FIDL绑定将自动生成并以各种语言进行类型检查。
<!-- 
**libfdio** contains both the client and server-side code for the C/C++
implementation of RIO, and is responsible for cautiously verifying the input
and output of both ends. -->

**libfdio**包含了用于RIO的C/C++实现的客户端和服务端代码，并负责谨慎地验证这两端的输入和输出。

<!-- In the case of the `open` operation, the remote IO protocol expects that the
client will create a channel and pass one end (as a handle) to the server. Once
the transaction is complete, this channel may be used as the mechanism to
communicate with the opened file, just as there had previously been a
communication with the “CWD” handle. -->
在“open”操作的例子中，Remote IO协议期望客户端将创建channel，并将channel的一端（作为句柄）传递给服务端。一旦事务完成，此channel可以用作与打开的文件进行通信的机制，就像之前与“CWD”句柄进行通信那样。

<!-- By designing the protocol so RIO clients provide handles, rather than servers,
the communication is better suited to pipelining. Access to RIO objects can be
asynchronous; requests to the RIO object can be transmitted before the object
is actually opened. This behavior is critical for interaction with services
(which will be described in more detail in the “ServiceFS” section). -->
通过协议的设计，RIO的客户端而不是服务端，来提供句柄，这样的通信方式更适合于流水线。访问RIO对象可以是异步的; RIO对象的请求可以在对象实际被打开之前被传输。这种行为对于与服务交互很关键（这部分将在“ServiceFS”部分详细介绍）。
<!-- 
To recap, an “open” call has gone through the standard library, acted on the
“CWD” fdio object, which transformed the request into a RIO message which is
sent to the server using the `zx_channel_write` system call. The client can
optionally wait for the server’s response using `zx_object_wait_one`, or continue
processing asynchronously. Either way, a channel has been created, where
one end lives with the client, and the other end is transmitted to the
“server". -->

总而言之，一次“open”调用已经经过标准库，并作用于“CWD”的fdio对象，该对象将请求转换为RIO消息，使用`zx_channel_write`系统调用将该消息发送到服务器。 客户端可以选择使用`zx_object_wait_one`等待服务器的响应，或者以异步方式继续运行。无论哪种方式，都创建了一个channel，其中一端与客户端共存，另一端传输到“服务端”。
<!-- 
## RemoteIO: Server-Side -->
## RemoteIO：服务端

<!-- ### Dispatching -->
### 分派

<!-- Once the message has been transmitted from the client’s side of the channel, it
lives in the server’s side of the channel, waiting to be read. The server is
identified by “whoever holds the handle to the other end of the channel” -- it
may live in the same (or a different) process as the client, use the same (or a
different) runtime than the client, and be written in the same (or a different
language) than the client. By using an agreed-upon wire-format, the
interprocess dependencies are bottlenecked at the thin communication layer that
occurs over channels. -->

一旦消息从channel的客户端发送出去，它就将驻留在channel的服务端并等待被读取。服务段由“持有channel另一端的人员”来标识 -- 它可能与客户端位于同一个（或不同的）进程中，使用与客户端相同（或不同）的运行时，并且使用与客户相同的（或不同的编程语言）实现。通过使用商定的编排格式，进程间依赖性在channel上发生，并在精简通信层上成为瓶颈。


<!-- At some point in the future, this server-side end of the CWD handle will need
to read the message transmitted by the client. This process isn’t automatic --
the server will need to intentionally wait for incoming messages on the
receiving handle, which in this case was the “current working directory”
handle. When server objects (files, directories, services, etc) are opened,
their handles are registered with a server-side Zircon **port** that waits for
their underlying handles to be **readable** (implying a message has arrived) or
**closed** (implying they will never receive more messages). This object which
dispatches incoming requests to appropriate handles is known as the dispatcher;
it is responsible for redirecting incoming messages to a callback function,
along with some previously-supplied “iostate” representing the open connection. -->

在未来的某个时刻，CWD句柄的服务端将需要读取客户端传输的消息。该过程不是自动完成的：服务端将需要有意等待接收句柄上的传入消息，在这种情况下，这是“当前工作目录”句柄。当服务端对象（文件，目录，服务等）被打开时，它们的句柄被注册到服务器端的Zircon**端口**，等待它们的底层句柄变成**可读**状态（意味着消息已经到达）或**关闭**（意味着他们永远不会收到更多的消息）。将传入请求分派给适当句柄的这个对象被称为调度器，它负责将传入的消息重定向到回调函数，以及一些以前提供的表示打开连接的“iostate”。

<!-- For C++ filesystems using libfs, this callback function is called
`vfs_handler`, and it receives a couple key pieces of information:

  * The RIO message which was provided by the client (or artificially constructed
    by the server to appear like a “close” message, if the handle was closed)
  * The I/O state representing the current connection to the handle (passed as the
    “iostate” field, mentioned earlier). -->

对于使用libfs的C ++文件系统，这个回调函数被称为`vfs_handler`，它接收几条关键信息：
   * 由客户端提供的RIO消息（或如果句柄已关闭并显示为“关闭”消息时，由服务器人为地构造）
   * 表示当前连接到句柄的I/O状态（以前面提到的“iostate”字段进行传递）。
<!-- 
`vfs_handler` can interpret the I/O state to infer additional information:

  * The seek pointer within the file (or within the directory, if readdir has been used)
  * The flags used to open the underlying resource
  * The Vnode which represents the underlying object (and may be shared between
    multiple clients, or multiple file descriptors) -->

`vfs_handler`可以通过解释I/O状态来推断附加信息：
  * 文件中的seek指针（或如果使用了readdir，在目录中的seek指针）
  * 用于打开底层资源的标志
  * 代表底层对象的Vnode（并且可以在多个客户端或多个文件描述符之间共享）

<!-- 
This handler function, equipped with this information, acts as a large
“switch/case” table, redirecting the RIO message to an appropriate function
depending on the “operation” field provided by the client. In the open case, the
`ZXRIO_OPEN` field is noticed as the operation, so (1) a handle is expected, and
(2) the ‘data’ field (“foo”) is interpreted as the path. -->

该处理函数配备了这些信息，充当了一个大型的“switch/case”表，根据客户端提供的“operation”字段将RIO消息重定向到一个合适的函数。在开放的情况下，`ZXRIO_OPEN`字段被认定为操作，因此（1）需要一个句柄，并且（2）'data'字段（“foo”）被解释为路径。

<!-- ### VFS Layer -->
### VFS层
<!-- 
In Fuchsia, the “VFS layer” is a filesystem-independent library of code which
may dispatch and interpret server-side messages, and call operations in the
underlying filesystem where appropriate. Notably, this layer is completely
optional -- if a filesystem server does not want to link against this library,
they have no obligation to use it. To be a filesystem server, a process must
merely understand the remote IO wire format. As a consequence, there could be
any number of “VFS” implementations in a language, but at the time of writing,
two well-known implementations exist: one written in C++ within the [libfs
library](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/fs/),
and another written in Go in the [rpc package of
ThinFS](https://fuchsia.googlesource.com/garnet/+/master/go/src/thinfs/zircon/rpc/rpc.go)] -->
在Fuchsia中，“VFS层”是独立于文件系统的代码库，它可以分派和解释服务端消息，并在适当的地方调用底层文件系统中的操作。值得注意的是，该层是完全可选的：如果文件系统服务器不想链接到这个库，它们便没有义务使用它。要成为文件系统服务器，进程仅必须了解Remote IO的编排格式，因此，一种编程语言中尽管可以有任意数量的“VFS”实现，但在撰写本文时已有两个众所周知的实现：用C++编写实现的[libfs库](https://github.com/fuchsia-mirror/zircon/tree/master/system/ulib/fs)，另一个是在Go中用编写的[ThinFS的rpc包](https://github.com/fuchsia-mirror/garnet/blob/master/go/src/thinfs/zircon/rpc/rpc.go)。

<!-- 
The VFS layer defines the interface of operations which may be routed to the
underlying filesystem, including:

  * Read/Write to a Vnode
  * Lookup/Create/Unlink a Vnode (by name) from a parent Vnode
  * Rename/Link a Vnode by name
  * And many more -->
VFS层定义了可从客户端路由到底层文件系统的操作接口，这其中包括：
  * 读/写Vnode
  * 从父Vnode查找/创建/取消链接Vnode（按名称）
  * 按名称重命名/链接Vnode
  * 以及更多……
<!-- 
To implement a filesystem (assuming a developer wants to use the shared VFS
layer), one simply needs to define a Vnode implementing this interface and link
against a VFS layer. This will provide functionality like “path walking” and
“filesystem mounting” with minimal effort, and almost no duplicated code. In an
effort to be filesystem-agnostic, the VFS layer has no preconceived notion of
the underlying storage used by the filesystem: filesystems may require access
to block devices, networks, or simply memory to store data -- but the VFS layer
only deals with interfaces acting on paths, byte arrays of data, and vnodes. -->

要实现文件系统（假设开发人员想要使用共享的VFS层），只需定义一个实现此接口的Vnode并链接到VFS层。这将以最小的努力提供诸如“路径漫游”和“文件系统安装”等功能，并且几乎不会有重复的代码。为了保持文件系统无关性，VFS层没有先入为主的文件系统所使用的底层存储的概念：文件系统可能需要访问块设备，网络或存储数据的内存；但VFS层只处理作用于路径的接口，字节数据数组和Vnode。

<!-- ### Path Walking -->
### 路径游走（Path Walking）

<!-- To open a server-side resource, the server is provided some starting point
(represented by the called handle) and a string path. This path is split into
segments by the “/” character, and each component is “looked up” with a
callback to the underlying filesystem. If the lookup successfully returns a
vnode, and another “/” segment is detected, then the process continues until
(1) `lookup` failsto find a component, (2) path processing reaches the last
component in a path, or (3) `lookup` finds a **mountpoint vnode**, which is a
vnode that has an attached “remote” handle. For now, we will ignore mountpoint
vnodes, although they are discussed in a section on [filesystem
mounting](filesystems.md#Mounting). -->

要打开服务器端资源，服务器会提供一些起始点（由被调用句柄表示）和一个字符串路径，该路径被“/”字符分割成段，并且每个组件都通过对底层文件系统的回调来进行“查找”。如果查找成功则返回vnode，并且检测到另一个“/”段，则该过程继续直到（1）`lookup`找不到组件，（2）路径处理到达路径中的最后一个组件，或者（3） `lookup`找到一个**vnode挂载点**，这是一个具有附加的“远程”句柄的vnode。 现在，我们将忽略挂载点vnode，尽管它们在[文件系统挂载](filesystems.md#挂载)一节中讨论。

<!-- Let’s assume `lookup` successfully found the “foo” Vnode. The filesystem server
will proceed to call the VFS interface “Open”, verifying that the requested
resource can be accessed with the provided flags, before calling “GetHandles”
asking the underlying filesystem if there are additional handles required to
interact with the Vnode. Assuming the client asked for the “foo” object
synchronously (which is implied with the default POSIX open call), any
additional handles required to interact with “foo” are packed into a small RIO
description object and passed back to the client. Alternatively, if "foo" had
failed to open, a RIO discription object would still be returned, but with the
“status” field set to an error code, indicating failure. Let’s assume the “foo”
open was successful. The server will proceed to create an “iostate” object for
“foo” and register it with the dispatcher. Doing so, future calls to “foo” can
be handled by the server. “Foo” has been opened, the client is now ready to send
additional requests. -->

现在假设`lookup`已成功找到了名为“foo”的Vnode。文件系统服务器将继续调用VFS接口的“Open”，验证是否可以使用提供的标志访问所请求的资源，然后调用“GetHandles”询问底层文件系统是否有需要与Vnode交互的其他句柄。假设客户端同步询问“foo”对象（这是POSIX中open调用默认的方式所隐含的），与“foo”交互所需的任何附加句柄都被打包到一个小型RIO描述对象中并传回客户端。或者，如果“foo”未能打开，RIO描述对象仍会返回，但“status”字段设置为错误代码，表示失败。假设“foo”打开成功。服务器将继续为“foo”创建一个“iostate”对象并将其注册到调度程序。这样做，未来对“foo”的调用可以由服务端来处理。至此，“Foo”已经打开，客户现在便可发送额外的请求。
<!-- 
From the client’s perspective, at the start of the “Open” call, a path and
handle combination was transmitted over the CWD handle to a remote filesystem
server. Since the call was synchronous, the client proceeded to wait for a
response on the handle. Once the server properly found, opened, and initialized
I/O state for this file, it sent back a “success” RIO description object. This
object would be read by the client, identifying that the call completed
successfully. At this point, the client could create an fdio object
representing the handle to “foo”, reference it with an entry in a file
descriptor table, and return the fd back to whoever called the original “open”
function. Furthermore, if the client wants to send any additional requests
(such as “read” or “write”) to ‘foo’, then they can communicate directly with
the filesystem server by using the connection to the opened file -- there is no
need to route through the ‘CWD’ on future requests. -->

从客户端的角度来看，在“Open”调用开始时，通过CWD句柄将路径和句柄组合传输到远程文件系统服务器。由于调用是同步的，客户端继续等待句柄上的响应。 一旦服务器正确找到，打开并初始化此文件的I/O状态，它就会发回一个“success”的RIO描述对象。该对象将被客户端读取，表明该调用已成功完成。此时，客户端可以创建一个表示“foo”句柄的fdio对象，将其引用到文件描述符表中的条目中，并将fd返回给称为原始“open”功能的任何对象。此外，如果客户想要发送任何额外的请求（比如“read”或“write”）到'foo'，那么他们可以通过使用与打开的文件的连接直接与文件系统服务器通信，而这不需要在未来的请求中路由到'CWD'。

## `Open`的生命周期: 图解

```
             +----------------+
             | Client Program |
+-----------------------------+
|   fd: x    |   fd: y    |
| Fdio (RIO) | Fdio (RIO) |
+-------------------------+
| '/' Handle | CWD Handle |
+-------------------------+
      ^            ^
      |            |
Zircon Channels, speaking RIO                   State BEFORE open(‘foo’)
      |            |
      v            v
+-------------------------+
| '/' Handle | CWD Handle |
+-------------------------+
|  I/O State |  I/O State |
+-------------------------+
|   Vnode A  |   Vnode B  |
+------------------------------+
           | Filesystem Server |
           +-------------------+


             +----------------+
             | Client Program |
+-----------------------------+
|   fd: x    |   fd: y    |
| Fdio (RIO) | Fdio (RIO) |
+-------------------------+
| '/' Handle | CWD Handle |   **foo Handle x2**
+-------------------------+
      ^            ^
      |            |
Zircon Channels, speaking RIO                   Client Creates Channel
      |            |
      v            v
+-------------------------+
| '/' Handle | CWD Handle |
+-------------------------+
|  I/O State |  I/O State |
+-------------------------+
|   Vnode A  |   Vnode B  |
+------------------------------+
           | Filesystem Server |
           +-------------------+


             +----------------+
             | Client Program |
+-----------------------------+
|   fd: x    |   fd: y    |
| Fdio (RIO) | Fdio (RIO) |
+-------------------------+--------------+
| '/' Handle | CWD Handle | ‘foo’ Handle |
+-------------------------+--------------+
      ^            ^
      |            |
Zircon Channels, speaking RIO                   Client Sends RIO message to Server
      |            |                            Message includes a ‘foo’ handle
      v            v                            (and waits for response)
+-------------------------+
| '/' Handle | CWD Handle |
+-------------------------+
|  I/O State |  I/O State |
+-------------------------+
|   Vnode A  |   Vnode B  |
+------------------------------+
           | Filesystem Server |
           +-------------------+


             +----------------+
             | Client Program |
+-----------------------------+
|   fd: x    |   fd: y    |
| Fdio (RIO) | Fdio (RIO) |
+-------------------------+--------------+
| '/' Handle | CWD Handle | ‘foo’ Handle |
+-------------------------+--------------+
      ^            ^
      |            |
Zircon Channels, speaking RIO                   Server dispatches message to I/O State,
      |            |                            Interprets as ‘open’
      v            v                            Finds or Creates ‘foo’
+-------------------------+
| '/' Handle | CWD Handle |
+-------------------------+
|  I/O State |  I/O State |
+-------------------------+-------------+
|   Vnode A  |   Vnode B  |   Vnode C   |
+------------------------------+--------+
           | Filesystem Server |
           +-------------------+


             +----------------+
             | Client Program |
+-----------------------------+
|   fd: x    |   fd: y    |
| Fdio (RIO) | Fdio (RIO) |
+-------------------------+--------------+
| '/' Handle | CWD Handle | ‘foo’ Handle |
+-------------------------+--------------+
      ^            ^          ^
      |            |          |
Zircon Channels, speaking RIO |                  Server allocates I/O state for Vnode
      |            |          |                  Responds to client-provided handle
      v            v          v
+-------------------------+--------------+
| '/' Handle | CWD Handle | ‘foo’ Handle |
+-------------------------+--------------+
|  I/O State |  I/O State |  I/O State   |
+-------------------------+--------------+
|   Vnode A  |   Vnode B  |    Vnode C   |
+------------------------------+---------+
           | Filesystem Server |
           +-------------------+


             +----------------+
             | Client Program |
+-----------------------------+----------+
|   fd: x    |   fd: y    |    fd: z     |
| Fdio (RIO) | Fdio (RIO) |  Fdio (RIO)  |
+-------------------------+--------------+
| '/' Handle | CWD Handle | ‘foo’ Handle |
+-------------------------+--------------+
      ^            ^          ^
      |            |          |
Zircon Channels, speaking RIO |                  Client recognizes that ‘foo’ was opened
      |            |          |                  Allocated Fdio + fd, ‘open’ succeeds.
      v            v          v
+-------------------------+--------------+
| '/' Handle | CWD Handle | ‘foo’ Handle |
+-------------------------+--------------+
|  I/O State |  I/O State |  I/O State   |
+-------------------------+--------------+
|   Vnode A  |   Vnode B  |    Vnode C   |
+------------------------------+---------+
           | Filesystem Server |
           +-------------------+
```
<!-- # Filesystem Architecture -->
# 文件系统架构
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/f870f0ec91c81c83208425c865ab349abc71fd09/the-book/filesystems.md)

---

<!-- This document seeks to describe a high-level view of the Fuchsia filesystems,
from their initialization, discussion of standard filesystem operations (such as
Open, Read, Write, etc), and the quirks of implementing user-space filesystems
on top of a microkernel. Additionally, this document describes the VFS-level
walking through a namespace which can be used to communicate with non-storage
entities (such as system services). -->

本文旨在描述Fuchsia文件系统的高层次视图，包括文件系统的初始化和标准操作（如Open、Read、Write等）的讨论，以及在微内核上实现用户空间文件系统的特殊行为。此外，本文档描述了通过VFS层的命名空间访问，该访问可用于与非存储实体（例如系统服务）进行通信。

<!-- ## Filesystems are Services -->
## 文件系统即服务

<!-- Unlike more common monolithic kernels, Fuchsia’s filesystems live entirely
within userspace. They are not linked nor loaded with the kernel; they are
simply userspace processes which implement servers that can appear as
filesystems. As a consequence, Fuchsia’s filesystems themselves can be changed
with ease -- modifications don’t require recompiling the kernel. In fact,
updating to a new Fuchsia filesystem can be done without rebooting. -->

与更常见的宏内核不同，Fuchsia的文件系统完全位于用户空间内，它没有链接到内核也没有加载内核，而仅仅只是用户空间内的可以作为文件系统出现的服务进程。因此，Fuchsia的文件系统本身可以轻松地被更改——而修改不需要重新编译内核。实际上，无需重新启动即可更新到新的Fuchsia文件系统。

<!-- 
Like other native servers on Fuchsia, the primary mode of interaction with a
filesystem server is achieved using the handle primitive rather than system
calls. The kernel has no knowledge about files, directories, or filesystems. As
a consequence, filesystem clients cannot ask the kernel for “filesystem access”
directly. -->

与Fuchsia上的其他本地服务一样，使用句柄原语而不是系统调用的方式成为与文件系统服务交互的主要模式，内核既不了解文件、目录，也不了解文件系统本身。因此，文件系统的客户程序无法直接向内核请求“访问文件系统”。

<!-- This architecture implies that the interaction with filesystems is limited to
the following interface: -->
因此，此架构意味着与文件系统的交互仅限于以下接口：

 <!-- * The messages sent on communication channels established with the filesystem
   server. These communication channels may be local for a client-side
   filesystem, or remote.
 * The initialization routine (which is expected to be configured heavily on a
   per-filesystem basis; a networking filesystem would require network access,
   persistent filesystems may require block device access, in-memory filesystems
   would only require a mechanism to allocate new temporary pages). -->

  * 在与文件系统服务建立的通信通道（channel）上发送的消息。对于客户端文件系统，这些通信信道可以是本地或远程的。
  * 初始化的例程（需要在每个文件系统的基础上进行大量配置：网络文件系统需要网络访问权限，持久化文件系统可能需要访问块设备，而内存文件系统只需要某机制来分配新的临时页面即可）。
  
<!-- As a benefit of this interface, any resources accessible via a channel can make
themselves appear like filesystems by implementing the expected protocols for
files or directories. For example, “serviceFS” (discussed in more detail later
in this document) allows for service discovery through a filesystem interface. -->
此接口设计的一个好处是，可通过通道访问的任何资源都可以通过实现文件或目录的预期协议的方式，使它们看起来像文件系统。例如，"serviceFS"（在本文档后面会更详细地讨论）允许通过文件系统接口进行服务发现。

<!-- ## File Lifecycle -->
## 文件的生命周期

<!-- ### Establishing a Connection -->
### 建立连接
<!-- To open a file, Fuchsia programs (clients) send RPC requests to filesystem
servers using a protocol called [RemoteIO](life_of_an_open.md#RemoteIO).
RemoteIO defines a wire-format for transmitting messages and handles between a
filesystem client and server. Instead of interacting with a kernel-implemented
VFS layer, Fuchsia processes send requests to filesystem services which
implement protocols for Files, Directories, and Devices. To send one of these
open requests, a Fuchsia process must transmit an RPC message over an existing
handle to a directory; for more detail on this process, refer to the [life of an
open document](life_of_an_open.md). -->

为了打开文件，Fuchsia应用程序（客户端）通过名为[RemoteIO](life_of_an_open.md＃remoteio)的协议将RPC请求发送到文件系统服务上。RemoteIO定义了用于在文件系统客户端和服务之间传输消息和句柄的有线格式。Fuchsia进程不是与内核实现的VFS层交互，而是向实现了文件，目录和设备的协议的文件系统服务发送请求。为了向其中之一的实体发送`Open`请求，Fuchsia进程必须通过现有句柄将RPC消息传输到目录上; 关于此过程的更多详细信息，请参阅[文件打开操作的生命周期](life_of_an_open.md)。

<!-- ### Namespaces -->
### 命名空间

<!-- On Fuchsia, a [namespace](namespaces.md) is a small filesystem which exists
entirely within the client. At the most basic level, the idea of the client
saving “/” as root and associating a handle with it is a very primitive
namespace. Instead of a typical singular "global" filesystem namespace, Fuchsia
processes can be provided an arbitrary directory handle to represent "root",
limiting the scope of their namespace. In order to limit this scope, Fuchsia
filesystems [intentionally do not allow access to parent directories via
dotdot](dotdot.md). -->

在Fuchsia上，[命名空间](namespaces.md)是一个小型的文件系统，它完全存在于客户端中。在最基本的层次上，客户程序以"root"形式保存"/"并将句柄与其关联的想法是一个非常原始的命名空间。可以为Fuchsia进程提供一个任意的目录句柄来表示"root"，而不是典型的单一“全局”文件系统命名空间，从而限制了其命名空间的范围。为了限制该作用域，Fuchsia文件系统[有意设计为不允许通过"dotdot"方式来访问父目录（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/dotdot.md)。
<!-- 
Fuchsia processes may additionally redirect certain path operations to separate
filesystem servers. When a client refers to “/bin”, the client may opt to
redirect these requests to a local handle representing the “/bin” directory,
rather than sending a request directly to the “bin” directory within the “root”
directory. Namespaces, like all filesystem constructs, are not visible from the
kernel: rather, they are implemented in client-side runtimes (such as
[libfdio](life_of_an_open.md#Fdio)) and are interposed between most client code
and the handles to remote filesystems. -->

Fuchsia进程还可以将某些路径操作重定向到单独的文件系统服务上。当客户端引用
"/bin"时，客户程序可以选择将这些请求重定向到表示"/bin"目录的本地句柄上，而不是直接向"root"目录中的"bin"目录发送请求。命名空间，像其他所有文件系统结构一样，对内核是不可见的：相反，它们被实现为客户端的运行时（例如[libfdio](life_of_an_open.md#fdio)）或，客户端代码和直接使用句柄操纵远程文件系统之间的部分。

<!-- Since namespaces operate on handles, and most Fuchsia resources and services
are accessible through handles, they are extremely powerful concepts.
Filesystem objects (such as directories and files), services, devices,
packages, and environments (visible by privileged processes) all are usable
through handles, and may be composed arbitrarily within a child process. As a
result, namespaces allows for customizable resource discovery within
applications. The services that one process observes within “/svc” may or may
not match what other processes see, and can be restricted or redirected
according to application-launching policy. -->

由于命名空间在句柄上操作，并且大多数Fuchsia资源和服务都可通过句柄访问，因此它们是非常强大的概念。文件系统对象（如目录和文件）、服务、设备、程序包和环境（对特权进程可见）都可以通过句柄来操纵，并可以在子进程中任意组合。因此，命名空间允许在应用程序中进行可自定义的资源发现，使得一个进程在"/svc"中看到的服务可能与其他进程看到的不一样，同时可以根据应用程序启的动策略进行限制或重定向。

<!-- For more detail the mechanisms and policies applied to restricting process
capability, refer to the documentation on
[sandboxing](sandboxing.md). -->
有关限制进程capability的机制和策略的更多详细信息，请参阅[沙箱](sandboxing.md)有关的文档。

<!-- ### Passing Data -->
### 传递数据

<!-- Once a connection has been established, either to a file, directory, device,
or service, subsequent operations are also transmitted using RPC messages.
These messages are transmitted on one or more handles, using a wire format that
the server validates and understands. -->


<!-- In the case of files, directories, and devices, these operations use the
RemoteIO protocol; in the case of services, these operations use the FIDL
protocol, though there are plans to unify all operations into the FIDL protocol. -->
建立连接后，无论是文件、目录、设备还是服务，都可以使用RPC消息传输后续操作。这些消息使用服务能够验证和理解的有线格式，在一个或多个句柄上传输。对于文件、目录和设备，这些操作使用RemoteIO协议，而在服务的情况下，这些操作则使用FIDL协议。尽管有计划将所有操作统一到FIDL协议中。

<!-- 
As an example, to seek within a file, a client would send an `ZXRIO_SEEK`
message with the desired position and “whence” within the RIO message, and the
new seek position would be returned. To truncate a file, an `FDIO_TRUNCATE`
message could be sent with the new desired filesystem, and a status message
would be returned. To read a directory, an `ZXRIO_READDIR` message could be
sent, and a list of direntries would be returned. If these requests were sent to
a filesystem entity that can’t handle them, an error would be sent, and the
operation would not be executed (like an `ZXRIO_READDIR` message sent to a text
file). -->

例如，为了在文件中执行seek操作，客户端需要在RIO消息中发送带有目标位置和"whence"的`ZXRIO_SEEK`消息，请求将返回新的seek位置。为了对文件执行truncate操作，可以对所需的新文件系统发送`FDIO_TRUNCATE`消息，请求将返回状态消息。为了读取目录，可发送`ZXRIO_READDIR`消息，请求将返回返回目录列表。如果将这些请求发送到无法处理它们的文件系统实体上（例如发送到文本文件的`ZXRIO_READDIR`消息），则并不会执行该操作，并返回错误消息。

<!-- ### Memory Mapping -->
### 内存映射

<!-- For filesystems capable of supporting it, memory mapping files is slightly more
complicated. To actually “mmap” part of a file, a client sends an “FDIO_MMAP”
message, and receives a Virtual Memory Object, or VMO, in response. This object
is then typically mapped into the client’s address space using a Virtual Memory
Address Region, or VMAR. Transmitting a limited view of the file’s internal
“VMO” back to the client requires extra work by the intermediate message
passing layers, so they can be aware they’re passing back a server-vendored
object handle. -->
对于能够提供支持的文件系统，内存映射文件稍微会复杂一些。为了能实际地`mmap`文件的一部分，客户端发送`FDIO_MMAP`消息，并接收虚拟内存对象（VMO）作为响应。而后，此对象通常通过虚拟内存地址区域（VMAR）映射文件到客户端的地址空间上。将文件内部"VMO"的有限视图传回客户端需要中间消息传递层的额外工作，因此它们可感知到它们正在传回服务产生的对象句柄。

<!-- By passing back these virtual memory objects, clients can quickly access the
internal bytes representing the file without actually undergoing the cost of a
round-trip IPC message. This feature makes mmap an attractive option for
clients attempting high-throughput on filesystem interaction. -->
通过传回这些虚拟内存对象，客户端可以快速访问文件的内部数据，而不会实际承受往返IPC消息的成本。此功能使得`mmap`成为试图在文件系统交互上实现高吞吐量客户端的有吸引力选项。

<!-- At the time of writing, on-demand paging is not supported by the
kernel, and has not been wired into filesystems. As a result, if a client
writes to a “memory-mapped” region, the filesystem cannot reasonably identify
which pages have and have not been touched. To cope with this restriction, mmap
has only been implemented on **read-only filesystems**, such as blobfs. -->
在撰写本文时，内核尚不支持按需分页，并且尚未应用到文件系统上。因此，如果客户端向“内存映射”区域写入，则文件系统无法正确地识别哪些页面是存在的和哪些页面尚未存在。为了应对这种限制，`mmap`只实现在**只读文件系统**上，例如blobfs。


<!-- ### Other Operations acting on paths -->
### 其他作用于路径上的操作

<!-- In addition to the “open” operation, there are a couple other path-based
operations worth discussing: “rename” and “link”. Unlike “open”, these
operations actually act on multiple paths at once, rather than a single
location. This complicates their usage: if a call to “rename(‘/foo/bar’,
‘baz’)” is made, the filesystem needs to figure out a way to: -->
除了`open`操作之外，还有一些值得讨论的基于路径的操作：`rename`和`link`。与`open`操作不同，这些操作实际上同时作用于多个路径上，而不是单个位置上。这使得它们的使用变得复杂：如果调用`rename('/foo/bar', 'baz')`，文件系统则需要找到一种可行的方法，以达到以下目的：
  <!-- * Traverse both paths, even when they have distinct starting points (which is the
    case this here; one path starts at root, and other starts at the CWD)
  * Open the parent directories of both paths
  * Operate on both parent directories and trailing pathnames simultaneously -->

  * 遍历两条路径，即使它们具有不同的起点（这里的情况是：一条路径从根目录开始，另一条路径从当前目录`CWD`开始）
  * 打开两个路径的父目录
  * 同时在父目录和尾路径名（即文件名）上进行操作
  
<!-- To satisfy this behavior, the VFS layer takes advantage of a Zircon concept
called “cookies”. These cookies allow client-side operations to store open
state on a server, using a handle, and refer to it later using that same
handles. Fuchsia filesystems use this ability to refer to one Vnode while
acting on the other. -->
为了满足这种行为，VFS层利用了一种名为"cookies"的Zircon概念，这些cookie允许客户端的操作使用句柄在服务器上存储对象的打开状态，而稍后可使用相同的句柄引用它。Fuchsia文件系统使用此功能来引用一个Vnode，同时作用于另一个之上。

<!-- These multi-path operations do the following: -->
这些多路径操作执行以下操作：

  <!-- * Open the parent source vnode (for “/foo/bar”, this means opening “/foo”)
  * Open the target parent vnode (for “baz”, this means opening the current
    working directory) and acquire a vnode token using the operation
    `IOCTL_DEVMGR_GET_TOKEN`, which is a handle to a filesystem cookie.
  * Send a “rename” request to the source parent vnode, along with the source
    and destination paths (“bar” and “baz”), along with the vnode token acquired
    earlier. This provides a mechanism for the filesystem to safely refer to the
    destination vnode indirectly -- if the client provides an invalid handle, the
    kernel will reject the request to access the cookie, and the server can return
    an error. -->
  * 打开源路径的父vnode（对于路径`/foo/bar`，则意味着打开`/foo`）
  * 打开目标路径的父vnode（对于`baz`，这意味着打开当前工作目录）并使用`IOCTL_DEVMGR_GET_TOKEN`操作获取vnode的token，该token是文件系统cookie的句柄。
  * 向源路径的父vnode发送`rename`请求，并同时发送相应的源路径和目标路径（`bar`和`baz`），以及先前获取的vnode token。这样就为文件系统间接安全地引用目标路径的vnode提供了一种机制——如果客户端提供无效句柄，内核将拒绝对cookie访问的请求，并从服务端返回错误。

<!-- ## Filesystem Lifecycle -->
## 文件系统生命周期

<!-- ### Mounting -->
### 挂载

<!-- When Fuchsia filesystems are initialized, they are created with typically two
handles: One handle to a channel used to communicate with the mounting
filesystem (referred to as the “mount point” channel -- the “mounting” end of
this channel is saved as a field named “remote” in the parent Vnode, the other
end will be connected to the root directory of the new filesystem), and
(optionally) another to contact the underlying [block device](block_devices.md).
Once a filesystem has been initialized (reading initial state off the block
device, finding the root vnode, etc) it flags a signal (`ZX_USER_SIGNAL0`) on
the mount point channel. This informs the parent (mounting) system that the
child filesystem is ready to be utilized. At this point, the channel passed to
the filesystem on initialization may be used to send filesystem requests, such
as “open”. -->
初始化Fuchsia文件系统时，通常使用两个句柄来创建它们：其中一个用于与挂载文件系统通信的通道（被称“挂载点”通道——此通道的"挂载"端保存为在父Vnode中名为`remote`的字段，另一端连接到新文件系统的根目录）句柄，以及（可选的）另一个和底层[block device](block_devices.md)交互的句柄。一旦文件系统初始化完成（从块设备读取初始状态，找到根vnode等），它就会在挂载点通道上标记一个信号（`ZX_USER_SIGNAL0`）。这将通知其（已挂载的）父文件系统：子文件系统已准备就绪可以使用。此时，在初始化时传递给文件系统的通道可用于发送文件系统请求，如"Open"等操作。

<!-- At this point, the parent (mounting) filesystem “pins” the connection to the
remote filesystem on a Vnode. The VFS layers capable of path walking check for
this remote handle when observing Vnodes: if a remote handle is detected, then
the incoming request (open, rename, etc) is forwarded to the remote filesystem
instead of the underlying node. If a user actually wants to interact with the
mountpoint node, rather than the remote filesystem, they can pass the
`O_NOREMOTE` flag to the “open” operation identify this intention. -->
此时，（已挂载的）父文件系统将与远程文件系统的连接“固定到”Vnode上。在观察Vnode时，可依路径行走的VFS层将检查此远程句柄：如果检测到远程句柄，则传入相应的请求（打开，重命名等）将转发到远程文件系统而不是底层的节点上。如果用户实际上想要与挂载节点而不是远程文件系统进行交互，它们可以将`O_NOREMOTE`标志传递给`open`操作作为标识。

<!-- Unlike many other operating systems, the notion of “mounted filesystems” does
not live in a globally accessible table. Instead, the question “what
mountpoints exist?” can only be answered on a filesystem-specific basis -- an
arbitrary filesystem may not have access to the information about what
mountpoints exist elsewhere. -->
与许多其他操作系统不同，"挂载文件系统"的概念并不存在于全局可访问表中。相反，“存在什么样的挂载点？”这样的问题只能在特定于文件系统的基础上进行回答——任意文件系统可能无法访问有关其他文件系统存在的挂载点信息。

<!-- ## Current Filesystems -->
## 当前的文件系统
<!-- Due to the modular nature of Fuchsia’s architecture, it is straightforward to
add filesystems to the system. At the moment, a handful of filesystems exist,
intending to satisfy a variety of distinct needs. -->
由于Fuchsia架构的模块化特性，将文件系统添加到操作系统中变得非常简单。目前，存在一些特定文件系统，旨在满足各种不同的需求。

<!-- ### MemFS: An in-memory filesystem -->
### MemFS：内存中的文件系统
<!-- 
[MemFS](https://fuchsia.googlesource.com/zircon/+/master/system/ulib/memfs)
is used to implement requests to temporary filesystems like `/tmp`, where files
exist entirely in RAM, and are not transmitted to an underlying block device.
This filesystem is also currently used for the “bootfs” protocol, where a
large, read-only VMO representing a collection of files and directories is
unwrapped into user-accessible Vnodes at boot (these files are accessible in
`/boot`). -->

[MemFS](https://github.com/fuchsia-mirror/zircon/tree/master/system/ulib/memfs)用于实现对`/tmp`这样的临时文件系统的请求。其中文件系统中的文件完全存在于RAM中，而不是传输到底层块设备上。此文件系统当前也用于"bootfs"协议，其中表示文件和目录集合的大型只读VMO在引导时会将其打包到用户可访问的vnode上（使得这些文件可在`/boot`中进行访问）。

<!-- ### MinFS: A persistent filesystem -->
### MinFS：持久化文件系统
<!-- 
[MinFS](https://fuchsia.googlesource.com/zircon/+/master/system/uapp/minfs/)
is a simple, traditional filesystem which is capable of storing files
persistently. Like MemFS, it makes extensive use of the VFS layers mentioned
earlier, but unlike MemFS, it requires an additional handle to a block device
(which is transmitted on startup to a new MinFS process). For ease of use,
MinFS also supplies a variety of tools: “mkfs” for formatting, “fsck” for
verification, as well as “mount” and “umount” for adding and subtracting MinFS
filesystems to a namespace from the command line. -->
[MinFS](https://github.com/fuchsia-mirror/zircon/tree/master/system/uapp/minfs)是一个简单的传统文件系统，能够持久化存储文件。与MemFS一样，它广泛使用前面提到的VFS层，但与MemFS不同的是，它需要一个块设备的附加句柄（在启动时传输到新MinFS进程上）。为了便于使用，MinFS还提供了各种工具：用于格式化的"mkfs"，用于验证的"fsck"，以及用于从命令行向命名空间添加或删除MinFS文件系统的"mount"和"umount"。

<!-- ### Blobfs: An immutable, integrity-verifying package storage filesystem -->
### Blobfs：一个不可变的，完整性验证的程序包存储文件系统

<!-- [Blobfs](https://fuchsia.googlesource.com/zircon/+/master/system/uapp/blobfs/)
is a simple, flat filesystem optimized for “write-once, then read-only” [signed
data](merkleroot.md), such as [application packages](package_metadata.md).
Other than two small prerequisites (file names which are deterministic, content
addressable hashes of a file’s Merkle Tree root, for integrity-verification)
and forward knowledge of file size (identified to Blobfs by a call to
“ftruncate” before writing a blob to storage), Blobfs appears like a
typical filesystem. It can be mounted and unmounted, it appears to contain a
single flat directory of hashes, and blobs can be accessed by operations like
“open”, “read”, “stat” and “mmap”. -->

[Blobfs](https://github.com/fuchsia-mirror/zircon/tree/master/system/uapp/blobfs)是一个简单，扁平化的文件系统，它针对“一次写入，然后只读”的[签名数据（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/merkleroot.md)，如[程序包（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/package_metadata.md)进行了优化。除了两个小的先决条件（文件名是确定性的，可寻址的文件Merkle树的根节点哈希值，其用于完整性验证）和文件大小的先决知识（在将blob写入存储体之前通过调用`ftruncate`识别为Blobfs）之外，Blobfs看起来像一个典型的文件系统。它可以被挂载和卸载，并包含单一扁平的哈希目录，并可以通过`open`，`read`，`stat`和`mmap`等操作访问blob。

<!-- ### ThinFS: A FAT filesystem written in Go -->
### ThinGS：用Go编写的FAT文件系统

<!-- [ThinFS](https://fuchsia.googlesource.com/garnet/+/master/go/src/thinfs/) is an implementation of a
FAT filesystem in Go. It serves a dual purpose: first, proving that our system
is actually modular, and capable of using novel filesystems, regardless of
language or runtime. Secondly, it provides a mechanism for reading a universal
filesystem, found on EFI partitions and many USB sticks. -->

[ThinFS](https://github.com/fuchsia-mirror/garnet/tree/master/go/src/thinfs)是FAT文件系统的一个Go语言实现。它的存在有两个目的：首先，证明了我们的系统实际上是模块化的，并能够使用新型的文件系统，而与其实现语言或运行时无关；其次，它提供了一种在EFI分区和许多USB存储器上读取通用文件系统的机制。

### FVM

TODO: smklein
# libc
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/a774512b9d926ee438a77ddc6a5f362b71e0cc4b/the-book/libc.md)

---

<!-- ## What do we mean by libc? -->
## 当我们谈论libc时我们在谈论什么？
<!--- 
On Posix-y systems, programs link against a library (either
dynamically or statically) called libc. This library provides the
functions defined by the C standard, as well as the runtime
environment for C programs. Many systems also define other
platform-specific interfaces in the same library. Many of these
interfaces are the preferred way for userspace to access kernel
functionality. For example, Posix-y systems have an `open` function in
their libc which calls an `open` system call. Sometimes these are
cross-platform standards, such as pthreads. Others are interfaces to
kernel-specific functionality, such as `epoll` or `kqueue`. In any
case, this library is present on the system itself and is a stable
interface. In constast, Windows does not provide a systemwide libc in
its stable win32 interface.
--->
在Posix系统上，所有程序（无论动态还是静态方式）必需链接的库被称为libc。该库提供了标准C定义的函数，以及C程序运行所依赖的运行环境。除此之外，许多系统还在libc中定义了其他平台相关的接口，他们中的许多接口是获取内核功能的首选方式。例如，Posix系统的libc中有名为`open`的函数，它的主要功能就是进行`open`系统调用。libc中的一部分是跨平台的标准，例如`pthread`，而其他的是内核相关的功能，如`epoll`或`kqueue`等。在任何情况下，libc都存在于系统中，并且提供稳定的接口。相反，Windows操作系统并没有在它稳定的win32接口上提供全系统的libc。

<!---
On Fuchsia the story is a bit different from Posix systems. First, the
Zircon kernel (Fuchsia's microkernel) does not provide a typical
Posix system call interface. So a Posix function like `open` can't
call a Zircon `open` syscall. Secondly, Fuchsia implements some parts
of Posix, but omits large parts of the Posix model. Most conspicuously
absent are signals, fork, and exec. Third, Fuchsia does not require
that programs use libc's ABI. Programs are free to use their own libc,
or to do without. However, Fuchsia does provide a libc.so which
programs can dynamically link, which provides implementations both of
the C standard library and of the parts of Posix Fuchsia supports, as
typical Posix systems do.
--->
而在Fuchsia操作系统上，情况和Posix系统有点不一样。首先，Zircon内核（Fuchsia的底层微内核）并未提供典型的Posix系统调用接口。因此，诸如`open`等Posix函数并不能直接进行`open`系统调用。其次，Fuchsia实现了部分的Posix函数，但是忽略了大部分的Posix模型。其中最显著的缺失是signal相关，fork和exec。再次，Fuchsia不要求程序使用libc的ABI接口，程序可以自由选择它们自己的libc，或完全不用libc。尽管如此，Fuchsia也提供了可供程序动态链接的libc.so，正如典型的Posix系统那样，libc.so同时实现了C标准库和Fuchsia所支持的那部分Posix函数，

<!---
## Piece by piece
--->
## 一点一滴（*译者注：Piece by piece是Kelly Clarkson的一张专辑名*）

<!---
This is a partial list of what is implemented (or not) in Fuchsia's
libc.
--->
下面是Fuchsia的libc所实现功能的（和未实现的）不完全列表。

<!---
### The C standard library
--->
### C标准库

<!---
Fuchsia's libc implements the C11 standard. In particular this
includes the threading-related interfaces such as threads (`thrd_t`)
and mutexes (`mtx_t`). A small handful of extensions are also in this
portion of the system to bridge the C11 structures, like a `thrd_t`,
to underlying kernel structures, like the `zx_handle_t` underlying it.
--->
Fuchsia的libc实现了C11标准。特别地，它包含线程相关的接口，例如threads（`thrd_t`）和mutexes（`mtx_t`）。一小部分旨在桥接到C11的数据结构的系统扩展，也在这部分中。例如`thrd_t`，它实际上是桥接到底层的，诸如`zx_handle_t`这样的数据结构。

### Posix
<!---
Posix defines a number of interfaces. These include (not
exhaustively): file I/O, BSD sockets, and pthreads.
--->
Posix定义了一部分的接口，包括（非完全）：文件I/O，BSD socket和pthread。

<!---
#### File I/O and BSD sockets
--->
#### 文件I/O和BSD socket

<!---
Recall that Zircon is a microkernel that is not in the business of
implementing file I/O. Instead, other Fuchsia userspace services
provide filesystems. libc itself defines weak symbols for Posix file
I/O functions such as `open`, `write`, and `fstat`. However, all these
calls simply fail. In addition to libc.so, programs can link the
fdio.so library. fdio knows how to speak to those other Fuchsia
services over
[Channel IPC][zircon-concepts-message-passing], and
provides a Posix-like layer for libc to expose. Sockets are similarly
implemented via fdio communicating with the userspace network stack.
--->
这里再次重申Zircon是微内核，因此它不直接实现文件I/O部分，而是由其他Fuchsia用户态提供文件系统。libc它本身为Posix定义了这些文件I/O的符号，如`open`，`write`和`fstat`等。但是，调用它们的所有操作将会失败。除了libc.so之外，程序可以链接到`fdio.so`。`fdio`知道如何通过[基于Channel的进程间通信][zircon-concepts-message-passing]和这些I/O相关的Fuchsia服务交互，并为libc暴露了和Posix相似的一层接口。类似地，socket也是通过fdio同用户空间网络栈通信的方式实现。

#### pthreads

<!--- 
Fuchsia's libc provides parts of the pthread standard. In particular,
the core parts of `pthread_t` (those that map straightforwardly onto
the corresponding C11 concepts) and synchronization primitives like
`pthread_mutex_t` are provided. Some details, like process-shared
mutexes, are not implemented. The implemented subset does not aim to
be comprehensive.
--->

Fuchsia的libc提供了部分的pthread标准。特别是提供了`pthread_t`的核心部分（即直接映射到C11相关的那些）和诸如`pthread_mutex_t`等同步原语。而像进程共享互斥量等细节却并未实现，因此Fuchsia实现的pthread子集并不全面。

#### Signals

<!---
Fuchsia does not have Unix-style signals. Zircon provides no way to
directly implement them (the kernel provides no way to cause another
thread to jump off its thread of execution). Fuchsia's libc does not,
therefore, have a notion of signal-safe functions, and is not
implemented internally to be aware of mechanisms like signals.
--->
Fuchsia中不包含Unix风格的信号，因为Zircon无法直接实现它们（内核没办法提供手段使得当前线程控制另一线程跳出其执行状态）。因此，Fuchsia的libc也不打算提供signal安全的功能，并且在实现时也不将signal等机制考虑在内。

<!---
Because of this fact, libc functions will not `EINTR`, and it is not
necessary for Fuchsia-only code to consider that case. However, it is
perfectly safe to do so. Fuchsia still defines the `EINTR` constant,
and code written for both Posix and Fuchsia may still have
`EINTR`-handling loops.
--->

基于这样的事实，libc的函数将无法产生`EINTR`（信号中断错误），仅包含基于Fuchsia的代码也不必考虑这种错误，相反即使考虑这种情况也是安全的。Fuchsia还是定义了`EINTR`变量，为Posix或Fuchsia而写的代码也同样可使用`EINTR`处理循环。

<!---
#### fork and exec
--->
#### fork和exec

<!---
Zircon does not have fork or exec. Instead, process creation is
provided by [launchpad](https://github.com/fuchsia-mirror/docs/blob/a774512b9d926ee438a77ddc6a5f362b71e0cc4b/the-book/launchpad.md). While Zircon has Process and
Thread objects, these are pretty raw and know nothing about
ELF. Launchpad knows how to turn an ELF and some initial state into a
running process.
--->
Zircon内核中没有fork和exec，进程的创建是由[launchpad](launchpad.md)来提供支持。尽管Zircon中有Process和Thread对象，但它们是无格式的，并且和ELF无任何关系，launchpad知道如何将ELF和一些初始状态转变成运行的进程。

<!--- 
[zircon-concepts-message-passing]: https://fuchsia.googlesource.com/zircon/+/master/docs/concepts.md#message-passing-sockets-and-channels
--->

[zircon-concepts-message-passing]: /zircon/docs/concepts.md#消息传递socket和channel
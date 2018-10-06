# Zircon Handles
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/599a37ad75ad6a3e7963a808e9f05c8fd6865749/docs/handles.md)

---
<!-- [TOC] -->
- [基础](#基础)
- [使用句柄](#使用句柄)
- [垃圾回收](#垃圾回收)
- [特别情况](#特别情况)
- [无效句柄和句柄重用](#无效句柄和句柄重用)
- [另见](#另见)

<!-- ## Basics -->
## 基础

<!-- Handles are kernel constructs that allows user-mode programs to
reference a kernel object. A handle can be thought as a session
or connection to a particular kernel object. -->
句柄是允许用户程序引用内核对象引用的一种内核结构，它可以被认为是与特定内核对象的会话或连接。

<!-- It is often the case that multiple processes concurrently access
the same object via different handles. However, a single handle
can only be either bound to a single process or be bound to
kernel. -->
通常情况，多个进程可通过不同的句柄同时访问同一个对象。 
但是，单个句柄只能绑定到单个进程或绑定到内核中。

<!-- When it is bound to kernel we say it's 'in-transit'. -->
当它被绑定到内核时，我们称其为“传输(in-transit)”状态。

<!-- In user-mode a handle is simply a specific number returned by
some syscall. Only handles that are not in-transit are visible
to user-mode. -->
在用户空间中，句柄简单地以某些系统调用返回的特定数字值表示。
并且只有非“传输”状态的句柄对用户空间是可见的。

<!-- The integer that represents a handle is only meaningful for that
process. The same number in another process might not map to any
handle or it might map to a handle pointing to a completely
different kernel object. -->
表示句柄的整型数值仅在该进程中是有意义的。 
另一个进程中的相同数字可能不表示任何句柄，也可能表示为指向完全不同的内核对象的句柄。

<!-- The integer value for a handle is any 32-bit number except
the value corresponding to ZX_HANDLE_INVALID. -->
表示句柄的整型值是除了`ZX_HANDLE_INVALID`对应的值之外的任何32位数。

<!-- For kernel-mode, a handle is a C++ object that contains three
logical fields: -->
在内核模式下，句柄是一个包含三个逻辑字段的C++对象：
<!-- + A reference to a kernel object
+ The rights to the kernel object
+ The process it is bound to (or if it's bound to kernel) -->
+ 内核对象的引用
+ 内核对象的权限
+ 它绑定的进程（或表示它绑定到内核）

<!-- The '[rights](rights.md)' specify what operations on the kernel object
are allowed. It is possible for a single process to have two different
handles to the same kernel object with different rights. -->
“[权限](rights.md)”指定了可以对内核对象执行哪些操作的特权。 
同一个进程可以为同一内核对象提供两个具有不同权限的句柄。

<!-- ## Using Handles -->
## 使用句柄

<!-- There are many syscalls that create a new kernel object
and which return a handle to it. To name a few: -->
有多个系统调用的功能是创建一个新的内核对象，并返回一个句柄。 
以下面几个例子为例：

+ [`zx_event_create`](syscalls/event_create.md)
+ [`zx_process_create`](syscalls/process_create.md)
+ [`zx_thread_create`](syscalls/thread_create.md)

<!-- These calls create both the kernel object and the first
handle pointing to it. The handle is bound to the process that
issued the syscall and the rights are the default rights for
that type of kernel object. -->
这些调用同时创建了内核对象和指向它的第一个句柄。 
句柄绑定到发出系统调用的进程中，权限是该类型内核对象的默认权限。

<!-- There is only one syscall that can make a copy of a handle,
which points to the same kernel object and is bound to the same
process that issued the syscall: -->
只有下列一个系统调用可以创建句柄的副本，新句柄指向同一个内核对象并绑定到发出系统调用的进程中：
+ [`zx_handle_duplicate`](syscalls/handle_duplicate.md)

<!-- 
There is one syscall that creates an equivalent handle (possibly
with fewer rights), invalidating the original handle: -->
另有一个系统调用可用于创建一个等效的句柄（可能具有较少的权限），并同时使原始句柄无效：
+ [`zx_handle_replace`](syscalls/handle_replace.md)

<!-- There is one syscall that just destroys a handle: -->
以下系统调用仅用于销毁一个句柄：
+ [`zx_handle_close`](syscalls/handle_close.md)

<!-- There are two syscalls that takes a handle bound to calling
process and binds it into kernel (puts the handle in-transit): -->
以下两个系统调用将一个句柄绑定到调用进程并将其绑定到内核（将句柄置于“传输”状态）：
+ [`zx_channel_write`](syscalls/channel_write.md)
+ [`zx_socket_share`](syscalls/socket_share.md)

<!-- There are three syscalls that takes an in-transit handle and
binds it to the calling process: -->
以下三个系统调用适用“传输”状态的句柄并将其绑定到调用进程：
+ [`zx_channel_read`](syscalls/channel_read.md)
+ [`zx_channel_call`](syscalls/channel_call.md)
+ [`zx_socket_accept`](syscalls/socket_accept.md)

<!-- The channel and socket syscalls above are used to transfer a handle from
one process to another. For example it is possible to connect
two processes with a channel. To transfer a handle the source process
calls `zx_channel_write` or `zx_channel_call` and then the destination
process calls `zx_channel_read` on the same channel. -->
上述的channel和socket系统调用用于将句柄从一个进程传输到另一个进程。 
例如，如果使用channel连接两个进程，源进程调用`zx_channel_write`或`zx_channel_call`发送句柄，然后目标进程在同一channel上调用`zx_channel_read`接收句柄。

<!-- Finally, there is a single syscall that gives a new process its
bootstrapping handle, that is, the handle that it can use to
request other handles: -->
最后，以下系统调用为新创建的进程提供引导句柄，即它是可以用来请求获取其他句柄的句柄：
+ [`zx_process_start`](syscalls/process_start.md)

<!-- 
The bootstrapping handle can be of any transferable kernel object but
the most reasonable case is that it points to one end of a channel
so this initial channel can be used to send further handles into the
new process. -->
引导句柄可以是任何可传输的内核对象，但最合理的使用方式是它指向channel的一端，使得通过该初始的channel可以将更多句柄发送到新进程中。

<!-- ## Garbage Collection -->
## 垃圾回收
<!-- 
If a handle is valid, the kernel object it points to is guaranteed
to be valid. This is ensured because kernel objects are ref-counted
and each handle holds a reference to its kernel object. -->
如果句柄有效，则可它指向的内核对象一定是有效的。 
这是可以保证的，因为内核对象采用引用计数(ref-count)的方式被追踪，而每个句柄都包含对其内核对象的引用。

<!-- The opposite does not hold. When a handle is destroyed it does not
mean its object is destroyed. There could be other handles pointing
to the object or the kernel itself could be holding a reference to
the kernel object. An example of this is a handle to a thread; the
fact that the last handle to a thread is closed it does not mean that
the thread has been terminated. -->
但相反的情况却并不成立。 
当句柄被销毁时，并不意味着它引用的对象一定会被销毁。 
可能有其他句柄指向该对象，或者内核本身可能持有对内核对象的引用。 
这其中一个例子是指向线程的句柄，线程的最后一个句柄被关闭并不意味着该线程将被终止。

<!-- When the last reference to a kernel object is released, the kernel
object is either destroyed or the kernel marks the object for
garbage collection; the object will be destroyed at a later time
when the current set of pending operations on it are completed. -->
当释放对内核对象的最后一个引用时，内核对象被销毁或者内核将该对象标记为可垃圾回收，当对象的当前排队中的操作全部完成以后将销毁该对象。

<!-- ## Special Cases -->
## 特别情况
<!-- + When a handle is in-transit and the channel or socket it was written
to is destroyed, the handle is closed.
+ Debugging sessions (and debuggers) might have special syscalls to
get access to handles. -->
+ 当句柄处于“传输”状态且它被写入的channel或socket被销毁时，该句柄将被关闭。
+ 调试会话（和调试器）可能具有特殊的系统调用来访问句柄。

<!-- ## Invalid Handles and handle reuse -->
## 无效句柄和句柄重用
<!-- It is an error to pass to any syscall except for `zx_object_get_info`
the following values: -->
传递以下句柄值给除`zx_object_get_info`以外的任何系统调用，都将产生错误：
<!-- 
+ A handle value that corresponds to a closed handle
+ The **ZX_HANDLE_INVALID** value, except for `zx_handle_close` syscall -->
+ 与已关闭句柄对应的句柄值
+ 对于除了`zx_handle_close`之外的其他系统调用而言，**ZX_HANDLE_INVALID**

<!-- The kernel is free to re-use the integer values of closed handles for
newly created objects. Therefore, it is important to make sure that proper
handle hygiene is observed: -->
内核可以自由地为新创建的对象重用已关闭句柄的整型值。 
因此，请务必确保遵守正确的句柄使用规则：

<!-- + Don't have one thread close a given handle and another thread use the
  same handle in a racy way. Even if the second thread is also closing it.
+ Don't ignore **ZX_ERR_BAD_HANDLE** return codes. They usually mean the
  code has a logic error. -->
+ 请勿在一个线程关闭给定的句柄时，另一个线程以竞争的方式使用相同的句柄，即使第二个线程也在关闭它。
+ 请勿忽略**ZX_ERR_BAD_HANDLE**的返回码，它们通常意味着代码存在逻辑错误。

<!-- Detecting invalid handle usage can be automated by using the
**ZX_POL_BAD_HANDLE** Job policy with **ZX_POL_ACTION_EXCEPTION** to
generate an exception when a process under such job object attempts any of
the of the mentioned invalid cases. -->
通过使用带有**ZX_POL_ACTION_EXCEPTION**标志位的**ZX_POL_BAD_HANDLE**作业策略，可以自动检测无效句柄的使用情况，以便使此类作业对象下的进程在尝试任何上述无效情况时产生异常。

<!-- ## See Also -->
## 另见

[Objects](objects.md),
[Rights](rights.md)
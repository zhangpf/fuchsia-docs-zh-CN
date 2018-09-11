# Thread（线程）
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/91786394a9cbb7b5ddd922ec761629eecd942203/docs/objects/thread.md)

---
<!-- ## NAME -->
## 名称

<!-- thread - runnable / computation entity -->
Thread —— 可运行/计算实体

<!-- ## SYNOPSIS -->
## 概览

TODO

<!-- ## DESCRIPTION -->
## 描述

<!-- The thread object is the construct that represents a time-shared CPU execution
context. Thread objects live associated to a particular
[Process Object](process.md) which provides the memory and the handles to other
objects necessary for I/O and computation. -->

线程对象是表示分时CPU执行上下文的概念。 
线程对象与特定的[进程对象](process.md)相关联，进程为其他对象所需的I/O和计算提供内存和句柄。

<!-- ### Lifetime -->
### 生命周期

<!-- Threads are created by calling `zx_thread_create()`, but only start executing
when either `zx_thread_start()` or `zx_process_start()` are called. Both syscalls
take as an argument the entrypoint of the initial routine to execute. -->
线程通过调用`zx_thread_create()`创建，但只有在调用`zx_thread_start()`或`zx_process_start()`时才开始执行。 
这两个系统调用都以初始例程的执行入口函数作为参数。

<!-- The thread passed to `zx_process_start()` should be the first thread to start execution
on a process. -->
传递给`zx_process_start()`的线程是在进程上第一个执行的线程。

<!-- A thread terminates execution: -->
线程可通过以下方式终止执行：
<!-- + by calling `zx_thread_exit()`
+ by calling `zx_vmar_unmap_handle_close_thread_exit()`
+ by calling `zx_futex_wake_handle_close_thread_exit()`
+ when the parent process terminates
+ by calling `zx_task_kill()` with the thread's handle
+ after generating an exception for which there is no handler or the handler
decides to terminate the thread. -->
+ 通过调用`zx_thread_exit()`
+ 通过调用`zx_vmar_unmap_handle_close_thread_exit()`
+ 通过调用`zx_futex_wake_handle_close_thread_exit()`
+ 在父进程终止时
+ 通过使用线程的句柄调用`zx_task_kill()`
+ 产生异常，但没有处理程序处理该异常或处理程序决定终止该线程时。

<!-- 
Returning from the entrypoint routine does not terminate execution. The last
action of the entrypoint should be to call `zx_thread_exit()` or one of the
above mentioned `_exit()` variants. -->
从入口例程返回时不会终止线程的执行。 
入口例程的最后一个动作应该是调用`zx_thread_exit()`或上面提到的`_exit()`变体之一。

<!-- 
Closing the last handle to a thread does not terminate execution. In order to
forcefully kill a thread for which there is no available handle, use
`zx_object_get_child()` to obtain a handle to the thread. This method is strongly
discouraged. Killing a thread that is executing might leave the process in a
corrupt state. -->
关闭线程的最后一个句柄不会终止它的执行。 
为了强制杀死没有句柄指向的线程，可使用`zx_object_get_child()`来获取线程的句柄。 
但强烈建议不要使用这种方法，因为杀死正在执行的线程可能会使进程处于损坏状态。

<!-- Fuchsia native threads are always *detached*. That is, there is no *join()* operation
needed to do a clean termination. However, some runtimes above the kernel, such as
C11 or POSIX might require threads to be joined. -->
Fuchsia原生线程总是处于*分离(detached)* 状态，也就是说，干净地终止该线程不需要执行*join()*操作。 
但是，某些内核之上的运行时（例如C11或POSIX）可能需要对线程执行`join`操作。

<!-- ## SYSCALLS -->
## 系统调用

<!-- + [thread_create](../syscalls/thread_create.md) - create a new thread within a process
+ [thread_exit](../syscalls/thread_exit.md) - exit the current thread
+ [thread_read_state](../syscalls/thread_read_state.md) - read register state from a thread
+ [thread_start](../syscalls/thread_start.md) - cause a new thread to start executing
+ [thread_write_state](../syscalls/thread_write_state.md) - modify register state of a thread -->
+ [thread_create](../syscalls/thread_create.md) —— 在进程中创建新线程
+ [thread_exit](../syscalls/thread_exit.md) —— 退出当前线程
+ [thread_read_state](../syscalls/thread_read_state.md) —— 从线程中读取寄存器状态
+ [thread_start](../syscalls/thread_start.md) —— 运行新线程
+ [thread_write_state](../syscalls/thread_write_state.md) —— 修改线程的寄存器状态

<br>

<!-- + [task_resume](../syscalls/task_resume.md) - cause a suspended task to continue running
+ [task_bind_exception_port](../syscalls/task_bind_exception_port.md) - attach an exception
port to a task
+ [task_kill](../syscalls/task_kill.md) - cause a task to stop running -->
+ [task_resume](../syscalls/task_resume.md) —— 继续执行挂起的任务
+ [task_bind_exception_port](../syscalls/task_bind_exception_port.md) —— 向任务上挂载异常端口
+ [task_kill](../syscalls/task_kill.md) —— 终止任务的运行

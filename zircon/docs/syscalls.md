<!---
# Zircon System Calls
--->
# Zircon系统调用
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls.md)

---

<!---
## Handles
+ [handle_close](syscalls/handle_close.md) - close a handle
+ [handle_duplicate](syscalls/handle_duplicate.md) - create a duplicate handle (optionally with reduced rights)
+ [handle_replace](syscalls/handle_replace.md) - create a new handle (optionally with reduced rights) and destroy the old one
--->
## Handle
+ [handle_close（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/handle_close.md) - 关闭句柄
+ [handle_duplicate（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/handle_duplicate.md) - 创建句柄副本（可选项为缩减的权限值）
+ [handle_replace（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/handle_replace.md) - 创建新句柄（可选项为缩减的权限值），并销毁旧句柄

<!---
## Objects
+ [object_get_child](syscalls/object_get_child.md) - find the child of an object by its koid
+ [object_get_cookie](syscalls/object_get_cookie.md) - read an object cookie
+ [object_get_info](syscalls/object_get_info.md) - obtain information about an object
+ [object_get_property](syscalls/object_get_property.md) - read an object property
+ [object_set_cookie](syscalls/object_set_cookie.md) - write an object cookie
+ [object_set_property](syscalls/object_set_property.md) - modify an object property
+ [object_signal](syscalls/object_signal.md) - set or clear the user signals on an object
+ [object_signal_peer](syscalls/object_signal.md) - set or clear the user signals in the opposite end
+ [object_wait_many](syscalls/object_wait_many.md) - wait for signals on multiple objects
+ [object_wait_one](syscalls/object_wait_one.md) - wait for signals on one object
+ [object_wait_async](syscalls/object_wait_async.md) - asynchronous notifications on signal change
--->
## Object
+ [object_get_child（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_get_child.md) - 通过koid，查找某个对象的子对象
+ [object_get_cookie（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_get_cookie.md) - 读取对象的cookie值
+ [object_get_info（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_get_info.md) - 获取对象的信息
+ [object_get_property（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_get_property.md) - 读取对象的属性值
+ [object_set_cookie（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_set_cookie.md) - 向对象的cookie中写入值
+ [object_set_property（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_set_property.md) - 修改对象属性值
+ [object_signal（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_signal.md) - 设置或清除某个对象上的用户信号
+ [object_signal_peer（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_signal.md) - 设置或清除相反对等方向上的用户信号
+ [object_wait_many（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_wait_many.md) - 等待多个对象上的信号产生
+ [object_wait_one（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_wait_one.md) - 等待单个对象上的信号产生
+ [object_wait_async（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/object_wait_async.md) - 在信号改变时进行异步通知

<!---
## Threads
+ [thread_create](syscalls/thread_create.md) - create a new thread within a process
+ [thread_exit](syscalls/thread_exit.md) - exit the current thread
+ [thread_read_state](syscalls/thread_read_state.md) - read register state from a thread
+ [thread_start](syscalls/thread_start.md) - cause a new thread to start executing
+ [thread_write_state](syscalls/thread_write_state.md) - modify register state of a thread
--->
## Thread
+ [thread_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/thread_create.md) - 在进程中创建新线程
+ [thread_exit（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/thread_exit.md) - 退出当前线程
+ [thread_read_state（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/thread_read_state.md) - 从线程中读取寄存器状态
+ [thread_start（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/thread_start.md) - 运行新线程
+ [thread_write_state（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/thread_write_state.md) - 修改线程的寄存器状态

<!---
## Processes
+ [process_create](syscalls/process_create.md) - create a new process within a job
+ [process_read_memory](syscalls/process_read_memory.md) - read from a process's address space
+ [process_start](syscalls/process_start.md) - cause a new process to start executing
+ [process_write_memory](syscalls/process_write_memory.md) - write to a process's address space
+ [process_exit](syscalls/process_exit.md) - exit the current process
--->
## Process
+ [process_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/process_create.md) - 在任务中创建新进程
+ [process_read_memory（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/process_read_memory.md) - 从进程的地址空间中读取数据
+ [process_start（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/process_start.md) - 运行新进程
+ [process_write_memory（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/process_write_memory.md) - 向进程的地址空间中写入数据
+ [process_exit（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/process_exit.md) - 退出当前进程

<!---
## Jobs
+ [job_create](syscalls/job_create.md) - create a new job within a job
+ [job_set_policy](syscalls/job_set_policy.md) - modify policies for a job and its descendants
+ [job_set_relative_importance](syscalls/job_set_relative_importance.md) - update a global ordering of jobs
--->
## Job
+ [job_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/job_create.md) - 在任务中创建新的任务
+ [job_set_policy（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/job_set_policy.md) - 改变任务的策略和它的子任务
+ [job_set_relative_importance（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/job_set_relative_importance.md) - 更新任务的全局排序

<!---
## Tasks (Thread, Process, or Job)
+ [task_bind_exception_port](syscalls/task_bind_exception_port.md) - attach an exception port to a task
+ [task_kill](syscalls/task_kill.md) - cause a task to stop running
+ [task_resume](syscalls/task_resume.md) - cause a suspended task to continue running
+ [task_suspend](syscalls/task_suspend.md) - cause a task to be suspended
--->
## 作业（Tasks），包括Thread, Process和Job
+ [task_bind_exception_port（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/task_bind_exception_port.md) - 向一个作业上挂载异常端口
+ [task_kill（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/task_kill.md) - 作业终止运行
+ [task_resume（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/task_resume.md) - 挂起的作业继续运行
+ [task_suspend（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/task_suspend.md) - 将一个作业中断挂起

<!---
## Channels
+ [channel_call](syscalls/channel_call.md) - synchronously send a message and receive a reply
+ [channel_create](syscalls/channel_create.md) - create a new channel
+ [channel_read](syscalls/channel_read.md) - receive a message from a channel
+ [channel_read_etc](syscalls/channel_read.md) - receive a message from a channel with handle information
+ [channel_write](syscalls/channel_write.md) - write a message to a channel
--->
## Channel
+ [channel_call](syscalls/channel_call.md) - 同步发送消息并接受响应
+ [channel_create](syscalls/channel_create.md) - 创建新channel
+ [channel_read](syscalls/channel_read.md) - 从channel中读取消息
+ [channel_read_etc](syscalls/channel_read.md) - 从channel中读取消息并返回带句柄信息的数组
+ [channel_write](syscalls/channel_write.md) - 向channel中读取消息

<!---
## Sockets
+ [socket_create](syscalls/socket_create.md) - create a new socket
+ [socket_read](syscalls/socket_read.md) - read data from a socket
+ [socket_write](syscalls/socket_write.md) - write data to a socket
--->
## Socket
+ [socket_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/socket_create.md) - 创建socket
+ [socket_read（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/socket_read.md) - 从socket中读数据
+ [socket_write（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/socket_write.md) - 向socket写入数据

<!---
## Fifos
+ [fifo_create](syscalls/fifo_create.md) - create a new fifo
+ [fifo_read](syscalls/fifo_read.md) - read data from a fifo
+ [fifo_write](syscalls/fifo_write.md) - write data to a fifo
--->
## 先进先出队列
+ [fifo_create](syscalls/fifo_create.md) - 创建队列
+ [fifo_read](syscalls/fifo_read.md) - 从队列中读取数据
+ [fifo_write](syscalls/fifo_write.md) - 写入数据到队列中

<!---
## Events and Event Pairs
+ [event_create](syscalls/event_create.md) - create an event
+ [eventpair_create](syscalls/eventpair_create.md) - create a connected pair of events
--->
## Event和Event Pair
+ [event_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/event_create.md) - 创建Event对象
+ [eventpair_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/eventpair_create.md) - 创建一对Event Pair

<!---
## Ports
+ [port_create](syscalls/port_create.md) - create a port
+ [port_queue](syscalls/port_queue.md) - send a packet to a port
+ [port_wait](syscalls/port_wait.md) - wait for packets to arrive on a port
+ [port_cancel](syscalls/port_cancel.md) - cancel notifications from async_wait
--->
## Port
+ [port_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_create.md) - 创建端口
+ [port_queue（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_queue.md) - 向端口发送数据包
+ [port_wait（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_wait.md) - 等待端口接收到数据包
+ [port_cancel（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/port_cancel.md) - 取消获取来自async_wait的通知

<!---
## Futexes
+ [futex_wait](syscalls/futex_wait.md) - wait on a futex
+ [futex_wake](syscalls/futex_wake.md) - wake waiters on a futex
+ [futex_requeue](syscalls/futex_requeue.md) - wake some waiters and requeue other waiters
--->

## Futex
+ [futex_wait（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/futex_wait.md) - 等待futex变成可用
+ [futex_wake（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/futex_wake.md) - 唤醒futex的等待者
+ [futex_requeue（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/futex_requeue.md) - 唤醒futex的一些等待者，并将其他等待者重新加入队列中

<!---
## Virtual Memory Objects (VMOs)
+ [vmo_create](syscalls/vmo_create.md) - create a new vmo
+ [vmo_read](syscalls/vmo_read.md) - read from a vmo
+ [vmo_write](syscalls/vmo_write.md) - write to a vmo
+ [vmo_clone](syscalls/vmo_clone.md) - clone a vmo
+ [vmo_get_size](syscalls/vmo_get_size.md) - obtain the size of a vmo
+ [vmo_set_size](syscalls/vmo_set_size.md) - adjust the size of a vmo
+ [vmo_op_range](syscalls/vmo_op_range.md) - perform an operation on a range of a vmo
--->
## 虚拟内存对象（VMO）
+ [vmo_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_create.md) - 创建vmo
+ [vmo_read（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_read.md) - 读取vmo
+ [vmo_write（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_write.md) - 写入vmo
+ [vmo_clone（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_clone.md) - 关闭vmo
+ [vmo_get_size（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_get_size.md) - 获取vmo的大小
+ [vmo_set_size（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_set_size.md) - 调整vmo的大小
+ [vmo_op_range（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmo_op_range.md) - 在vmo的一段区域内执行操作

<!---
## Virtual Memory Address Regions (VMARs)
+ [vmar_allocate](syscalls/vmar_allocate.md) - create a new child VMAR
+ [vmar_map](syscalls/vmar_map.md) - map a VMO into a process
+ [vmar_unmap](syscalls/vmar_unmap.md) - unmap a memory region from a process
+ [vmar_protect](syscalls/vmar_protect.md) - adjust memory access permissions
+ [vmar_destroy](syscalls/vmar_destroy.md) - destroy a VMAR and all of its children
--->
## 虚拟动态内存对象（VMAR）
+ [vmar_allocate（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_allocate.md) -创建子VMAR
+ [vmar_map（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_map.md) - 将VMO对象映射到某个进程中
+ [vmar_unmap（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_unmap.md) - 将某个内存区域从进程中取消映射
+ [vmar_protect（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_protect.md) - 调整内存访问的权限
+ [vmar_destroy（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_destroy.md) - 销毁VMAR和它所有的子代VMAR

<!---
## Cryptographically Secure RNG
+ [cprng_draw](syscalls/cprng_draw.md)
+ [cprng_add_entropy](syscalls/cprng_add_entropy.md)
--->
## 加密安全的随机数发生器
+ [cprng_draw（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/cprng_draw.md)
+ [cprng_add_entropy（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/cprng_add_entropy.md)

<!---
## Time
+ [nanosleep](syscalls/nanosleep.md) - sleep for some number of nanoseconds
+ [clock_get](syscalls/clock_get.md) - read a system clock
+ [ticks_get](syscalls/ticks_get.md) - read high-precision timer ticks
+ [ticks_per_second](syscalls/ticks_per_second.md) - read the number of high-precision timer ticks in a second
--->
## 时间
+ [nanosleep（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/nanosleep.md) - 休眠一段时间（以ns为单位）
+ [clock_get（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/clock_get.md) - 读取系统时钟
+ [ticks_get（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/ticks_get.md) - 读取高精度计时器滴答数
+ [ticks_per_second（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/ticks_per_second.md) - 读取一秒时间内高精度计时器滴答数

<!---
## Timers
+ [timer_create](syscalls/timer_create.md) - create a timer object
+ [timer_set](syscalls/timer_set.md) - start a timer
+ [timer_cancel](syscalls/timer_cancel.md) - cancel a timer
--->
## 计时器
+ [timer_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/timer_create.md) - 创建计时器对象
+ [timer_set（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/timer_set.md) - 启动计时器
+ [timer_cancel（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/timer_cancel.md) - 取消计时器

<!---
## Hypervisor guests
+ [guest_create](syscalls/guest_create.md) - create a hypervisor guest
+ [guest_set_trap](syscalls/guest_set_trap.md) - set a trap in a hypervisor guest
--->
## 虚拟机监视器管理的客户机
+ [guest_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/guest_create.md) - 创建虚拟客户机
+ [guest_set_trap（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/guest_set_trap.md) - 在虚拟机客户机中设置陷入中断

<!---
## Virtual CPUs
+ [vcpu_create](syscalls/vcpu_create.md) - create a virtual cpu
+ [vcpu_resume](syscalls/vcpu_resume.md) - resume execution of a virtual cpu
+ [vcpu_interrupt](syscalls/vcpu_interrupt.md) - raise an interrupt on a virtual cpu
+ [vcpu_read_state](syscalls/vcpu_read_state.md) - read state from a virtual cpu
+ [vcpu_write_state](syscalls/vcpu_write_state.md) - write state to a virtual cpu
--->
## 虚拟CPU
+ [vcpu_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vcpu_create.md) - 创建虚拟cpu
+ [vcpu_resume（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vcpu_resume.md) - 恢复虚拟cpu的继续运行
+ [vcpu_interrupt（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vcpu_interrupt.md) - 在虚拟cpu上产生中断
+ [vcpu_read_state（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vcpu_read_state.md) - 读取虚拟cpu的状态
+ [vcpu_write_state（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vcpu_write_state.md) - 向虚拟cpu写入状态信息

<!---
## Global system information
+ [system_get_features](syscalls/system_get_features.md) - get hardware-specific features
+ [system_get_num_cpus](syscalls/system_get_num_cpus.md) - get number of CPUs
+ [system_get_physmem](syscalls/system_get_physmem.md) - get physical memory size
+ [system_get_version](syscalls/system_get_version.md) - get version string
--->
## 全局系统信息
+ [system_get_features（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/system_get_features.md) - 获取硬件相关功能
+ [system_get_num_cpus（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/system_get_num_cpus.md) - 获取CPU核数
+ [system_get_physmem（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/system_get_physmem.md) - 获取物理内存大小
+ [system_get_version（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/system_get_version.md) - 获取版本信息字符串

<!---
## Logging
+ log_create - create a kernel managed log reader or writer
+ log_write - write log entry to log
+ log_read - read log entries from log
--->
## 日志
+ log_create - 创建内核托管的日志读写器
+ log_write - 向日志项中写入日志
+ log_read - 从日志项中读取日志

<!---
## Multi-function
+ [vmar_unmap_handle_close_thread_exit](syscalls/vmar_unmap_handle_close_thread_exit.md) - three-in-one
+ [futex_wake_handle_close_thread_exit](syscalls/futex_wake_handle_close_thread_exit.md) - three-in-one
--->
## 操作合并函数
+ [vmar_unmap_handle_close_thread_exit（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/vmar_unmap_handle_close_thread_exit.md) - 三合一操作（取消vmar映射，关闭句柄和退出线程）
+ [futex_wake_handle_close_thread_exit（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/futex_wake_handle_close_thread_exit.md) - 三合一操作（唤醒futex，关闭句柄和退出线程）

<!---
## DDK
+ [cache_flush](syscalls/cache_flush.md) - Flush CPU data and/or instruction caches
+ [interrupt_create](syscalls/interrupt_create.md) - Create an interrupt object
+ [interrupt_bind](syscalls/interrupt_bind.md) - Bind an interrupt vector to interrupt object
+ [interrupt_wait](syscalls/interrupt_wait.md) - Wait for an interrupt on an interrupt object
+ [interrupt_get_timestamp](syscalls/interrupt_get_timestamp.md) - Get the timestamp for an interrupt
+ [interrupt_signal](syscalls/interrupt_signal.md) - Signals a virtual interrupt on an interrupt object
+ acpi_uefi_rsdp
+ mmap_device_io
+ set_framebuffer
+ set_framebuffer_vmo
+ vmo_create_contiguous
+ vmo_create_physical
--->

## 驱动开发工具
+ [cache_flush（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/cache_flush.md) - 刷新CPU数据和（或）指令缓存
+ [interrupt_create（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/interrupt_create.md) - 创建中断对象
+ [interrupt_bind（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/interrupt_bind.md) - 绑定中断向量到中断对象上
+ [interrupt_wait（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/interrupt_wait.md) - 等待中断向量上发生中断
+ [interrupt_get_timestamp（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/interrupt_get_timestamp.md) - 获取中断的时间戳
+ [interrupt_signal（英文原文）](https://raw.githubusercontent.com/fuchsia-mirror/zircon/3adf3875541d28ad944637f753f8e454fa91dceb/docs/syscalls/interrupt_signal.md) - 向中断对象发送虚拟中断信号
+ acpi_uefi_rsdp
+ mmap_device_io
+ set_framebuffer
+ set_framebuffer_vmo
+ vmo_create_contiguous
+ vmo_create_physical
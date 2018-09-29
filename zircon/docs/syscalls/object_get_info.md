# zx_object_get_info
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_get_info.md)

---
<!-- ## NAME -->
## 名称

<!-- object_get_info - query information about an object -->
object_get_info —— 查询对象的相关信息

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/object.h>

zx_status_t zx_object_get_info(zx_handle_t handle, uint32_t topic,
                               void* buffer, size_t buffer_size,
                               size_t* actual, size_t* avail);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **object_get_info()** requests information about the provided handle (or the
object the handle refers to). The *topic* parameter indicates what specific
information is desired. -->
**object_get_info()** 请求获取提供的句柄（或句柄引用的对象）的相关信息。 
*topic*参数表示所需要的特定信息的主题。
<!-- 
*buffer* is a pointer to a buffer of size *buffer_size* to return the
information. -->
*buffer*是指向大小为*buffer_size*的缓冲区指针，用于存放返回的信息。

<!-- *actual* is an optional pointer to return the number of records that were
written to buffer. -->
*actual*是一个可选的指针，用于返回写入缓冲区的记录条数。
<!-- 
*avail* is an optional pointer to return the number of records that are
available to read. -->
*avail*是一个可选的指针，用于返回可读取的记录条数。

<!-- If the buffer is insufficiently large, *avail* will be larger than *actual*. -->
如果*buffer*缓冲区长度不够，那么*avail*将大于*actual*的值。

<!-- [TOC] -->
- [名称](#名称)
- [概要](#概要)
- [描述](#描述)
- [主题](#主题)
    - [ZX_INFO_HANDLE_VALID](#zx_info_handle_valid)
    - [ZX_INFO_HANDLE_BASIC](#zx_info_handle_basic)
    - [ZX_INFO_HANDLE_COUNT](#zx_info_handle_count)
    - [ZX_INFO_PROCESS_HANDLE_STATS](#zx_info_process_handle_stats)
    - [ZX_INFO_PROCESS](#zx_info_process)
    - [ZX_INFO_PROCESS_THREADS](#zx_info_process_threads)
    - [ZX_INFO_THREAD](#zx_info_thread)
    - [ZX_INFO_THREAD_EXCEPTION_REPORT](#zx_info_thread_exception_report)
    - [ZX_INFO_THREAD_STATS](#zx_info_thread_stats)
    - [ZX_INFO_CPU_STATS](#zx_info_cpu_stats)
    - [ZX_INFO_VMAR](#zx_info_vmar)
    - [ZX_INFO_VMO](#zx_info_vmo)
    - [ZX_INFO_SOCKET](#zx_info_socket)
    - [ZX_INFO_JOB_CHILDREN](#zx_info_job_children)
    - [ZX_INFO_JOB_PROCESSES](#zx_info_job_processes)
    - [ZX_INFO_TASK_STATS](#zx_info_task_stats)
    - [ZX_INFO_PROCESS_MAPS](#zx_info_process_maps)
    - [ZX_INFO_PROCESS_VMOS](#zx_info_process_vmos)
    - [ZX_INFO_KMEM_STATS](#zx_info_kmem_stats)
    - [ZX_INFO_RESOURCE](#zx_info_resource)
    - [ZX_INFO_BTI](#zx_info_bti)
- [权限](#权限)
- [返回值](#返回值)
- [错误码](#错误码)
- [示例](#示例)
- [另见](#另见)

<!-- ## TOPICS -->
## 主题

### ZX_INFO_HANDLE_VALID

<!-- *handle* type: **Any** -->
*handle*类型：**任意** 

<!-- *buffer* type: **n/a** -->
*buffer*类型：**n/a**

<!-- Returns **ZX_OK** if *handle* is valid, or **ZX_ERR_BAD_HANDLE** otherwise. No
records are returned and *buffer* may be NULL. -->
如果*handle*有效，则返回**ZX_OK**；否则返回**ZX_ERR_BAD_HANDLE**。 
该主题不返回任何记录，同时*buffer*可以为`NULL`。

### ZX_INFO_HANDLE_BASIC

<!-- *handle* type: **Any** -->
*handle*类型：**任意** 

<!-- *buffer* type: **zx_info_handle_basic_t[1]** -->
*buffer*类型：**zx_info_handle_basic_t[1]**

<!-- ```
typedef struct zx_info_handle_basic {
    // The unique id assigned by kernel to the object referenced by the
    // handle.
    zx_koid_t koid;

    // The immutable rights assigned to the handle. Two handles that
    // have the same koid and the same rights are equivalent and
    // interchangeable.
    zx_rights_t rights;

    // The object type: channel, event, socket, etc.
    uint32_t type;                // zx_obj_type_t;

    // If the object referenced by the handle is related to another (such
    // as the the other end of a channel, or the parent of a job) then
    // |related_koid| is the koid of that object, otherwise it is zero.
    // This relationship is immutable: an object's |related_koid| does
    // not change even if the related object no longer exists.
    zx_koid_t related_koid;

    // Set to ZX_OBJ_PROP_WAITABLE if the object referenced by the
    // handle can be waited on; zero otherwise.
    uint32_t props;               // zx_obj_props_t;
} zx_info_handle_basic_t;
``` -->

```
typedef struct zx_info_handle_basic {
    // 内核为句柄引用的对象分配的唯一ID。
    zx_koid_t koid;

    // 分配给句柄的不可变权限。 
    // 具有相同的koid和相同权限的两个句柄是等效和可互换的。
    zx_rights_t rights;

    // 对象类型：通道，事件，套接字等。
    uint32_t type;                // zx_obj_type_t;

    // 如果句柄引用的对象与另一个对象（例如通道的另一端或父级作业）相关，那么|related_koid| 是该对象的koid，否则为零。
    // 这种关系是不可变的：即使相关对象不再存在，对象的|related_koid|也不会改变。
    zx_koid_t related_koid;

    // 如果句柄引用的对象是可等待的，则设置为ZX_OBJ_PROP_WAITABLE；否则为零。
    uint32_t props;               // zx_obj_props_t;
} zx_info_handle_basic_t;
```

### ZX_INFO_HANDLE_COUNT

<!-- *handle* type: **Any** -->
*handle*类型：**任意** 

<!-- *buffer* type: **zx_info_handle_count_t[1]** -->
*buffer*类型：**zx_info_handle_count_t[1]**

<!-- ```
typedef struct zx_info_handle_count {
    // The number of outstanding handles to a kernel object.
    uint32_t handle_count;
} zx_info_handle_count_t;
``` -->
```
typedef struct zx_info_handle_count {
    // 内核对象被句柄引用的次数。
    uint32_t handle_count;
} zx_info_handle_count_t;
```
<!-- The *handle_count* should only be used as a debugging aid. Do not use it
to check that an untrusted processes cannot modify a kernel object. Due to
asynchronous nature of the system scheduler, there might be a time window
during which it is possible for an object to be modified by a previous handle
owner even as the last handle is transfered from one process to another. -->
*handle_count*应仅用作调试的辅助工具。 
不要使用它来检查不受信任的进程是否无法修改某个内核对象。 
由于系统调度程序是异步的，因此可能存在一个时间窗口：在该时间窗口期间，即使对象最后一个句柄从一个进程转移到另一个进程，也可以由先前的句柄所有者修改对象。

### ZX_INFO_PROCESS_HANDLE_STATS

<!-- *handle* type: **Process** -->
*handle*类型：**进程** 

<!-- *buffer* type: **zx_info_process_handle_stats_t[1]** -->
*buffer*类型：**zx_info_process_handle_stats_t[1]**

<!-- ```
typedef struct zx_info_process_handle_stats {
    // The number of outstanding handles to kernel objects of each type.
    uint32_t handle_count[ZX_OBJ_TYPE_LAST];
} zx_info_process_handle_stats_t;
``` -->
```
typedef struct zx_info_process_handle_stats {
    // 每种类型的内核对象被句柄引用的次数。
    uint32_t handle_count[ZX_OBJ_TYPE_LAST];
} zx_info_process_handle_stats_t;
```

### ZX_INFO_PROCESS

<!-- *handle* type: **Process** -->
*handle*类型：**进程** 

<!-- *buffer* type: **zx_info_process_t[1]** -->
*buffer*类型：**zx_info_process_t[1]**

<!-- ```
typedef struct zx_info_process {
    // The process's return code; only valid if |exited| is true.
    // Guaranteed to be non-zero if the process was killed by |zx_task_kill|.
    int64_t return_code;

    // True if the process has ever left the initial creation state,
    // even if it has exited as well.
    bool started;

    // If true, the process has exited and |return_code| is valid.
    bool exited;

    // True if a debugger is attached to the process.
    bool debugger_attached;
} zx_info_process_t;
``` -->
```
typedef struct zx_info_process {
    // 进程的返回码；仅在|exited|为true时有效。
    // 如果进程被|zx_task_kill|强制结束，则保证该字段为非零。
    int64_t return_code;

    // 如果进程已结束创建状态，则即使该进程已退出，该字段也为true。
    bool started;

    // 如果该字段为true，则进程已退出并且|return_code|有效。
    bool exited;

    // 如果调试器已附加到进程，则该字段为true。
    bool debugger_attached;
} zx_info_process_t;
```

### ZX_INFO_PROCESS_THREADS

<!-- *handle* type: **Process** -->
*handle*类型：**进程** 

<!-- *buffer* type: **zx_koid_t[n]** -->
*buffer*类型：**zx_koid_t[n]**
<!-- 
Returns an array of *zx_koid_t*, one for each running thread in the Process at
that moment in time. -->
返回*zx_koid_t*类型的数组，其中每一个对应于提供的进程句柄的其中一个线程。

<!-- N.B. Getting the list of threads is inherently racy.
This can be somewhat mitigated by first suspending all the threads,
but note that an external thread can create new threads.
*actual* will contain the number of threads returned in *buffer*.
*avail* will contain the total number of threads of the process at
the time the list of threads was obtained, it could be larger than *actual*. -->

注：获取线程列表会带来本质上的竞争。 
在此之前暂停所有线程可以稍微减轻这个问题，但请注意外部线程也可以创建新线程。
*actual*包含*buffer*中返回的线程数。 
*avail*包含获取线程列表时进程的线程总数，因此实际线程数可能大于*actual*。

### ZX_INFO_THREAD

<!-- *handle* type: **Thread** -->
*handle*类型：**线程** 

<!-- *buffer* type: **zx_info_thread_t[1]** -->
*buffer*类型：**zx_info_thread_t[n]**

<!-- ```
typedef struct zx_info_thread {
    // One of ZX_THREAD_STATE_* values.
    uint32_t state;

    // If |state| is ZX_THREAD_STATE_BLOCKED_EXCEPTION, the thread has gotten
    // an exception and is waiting for the exception to be handled by the
    // specified port.
    // The value is one of ZX_EXCEPTION_PORT_TYPE_*.
    uint32_t wait_exception_port_type;
} zx_info_thread_t;
``` -->
```
typedef struct zx_info_thread {
    // ZX_THREAD_STATE_*值之一。
    uint32_t state;

    // 如果|state|是ZX_THREAD_STATE_BLOCKED_EXCEPTION，线程在遇到异常时等待指定端口处理异常。
    // 该字段的值是ZX_EXCEPTION_PORT_TYPE_*之一。 
    uint32_t wait_exception_port_type;
} zx_info_thread_t;
```

<!-- The values in this struct are mainly for informational and debugging
purposes at the moment. -->
此结构中的值主要用于提供信息和调试目的。

<!-- The **ZX_THREAD_STATE_\*** values are defined by -->
**ZX_THREAD_STATE_\*** 值由以下头文件定义：
```
#include <zircon/syscalls/object.h>
```

<!-- *   *ZX_THREAD_STATE_NEW*: The thread has been created but it has not started running yet.
*   *ZX_THREAD_STATE_RUNNING*: The thread is running user code normally.
*   *ZX_THREAD_STATE_SUSPENDED*: Stopped due to [zx_task_suspend](task_suspend.md).
*   *ZX_THREAD_STATE_BLOCKED*: In a syscall or handling an exception.
    This value is never returned by itself.
	See **ZX_THREAD_STATE_BLOCKED_\*** below.
*   *ZX_THREAD_STATE_DYING*: The thread is in the process of being terminated,
    but it has not been stopped yet.
*   *ZX_THREAD_STATE_DEAD*: The thread has stopped running. -->
* *ZX_THREAD_STATE_NEW*：线程已创建，但尚未开始运行。
* *ZX_THREAD_STATE_RUNNING*：线程处于正常运行用户代码状态。
* *ZX_THREAD_STATE_SUSPENDED*：线程由于[zx_task_suspend](task_suspend.md)而停止。
* *ZX_THREAD_STATE_BLOCKED*：线程在系统调用或处理异常中。 
  该值永远不会自行返回。 
  请参阅下面的**ZX_THREAD_STATE_BLOCKED_\***。
* *ZX_THREAD_STATE_DYING*：线程正在终止，但尚未停止。
* *ZX_THREAD_STATE_DEAD*：线程已停止运行。

<!-- When a thread is stopped inside a blocking syscall, or stopped in an
exception, the value returned in **state** is one of the following: -->
当线程在阻塞型系统调用内停止或在异常中停止时，**state**中返回的值是以下之一：
<!-- *   *ZX_THREAD_STATE_BLOCKED_EXCEPTION*: The thread is stopped in an exception.
*   *ZX_THREAD_STATE_BLOCKED_SLEEPING*: The thread is stopped in [zx_nanosleep](nanosleep.md).
*   *ZX_THREAD_STATE_BLOCKED_FUTEX*: The thread is stopped in [zx_futex_wait](futex_wait.md).
*   *ZX_THREAD_STATE_BLOCKED_PORT*: The thread is stopped in [zx_port_wait](port_wait.md).
*   *ZX_THREAD_STATE_BLOCKED_CHANNEL*: The thread is stopped in [zx_channel_call](channel_call.md).
*   *ZX_THREAD_STATE_BLOCKED_WAIT_ONE*: The thread is stopped in [zx_object_wait_one](object_wait_one.md).
*   *ZX_THREAD_STATE_BLOCKED_WAIT_MANY*: The thread is stopped in [zx_object_wait_many](object_wait_many.md).
*   *ZX_THREAD_STATE_BLOCKED_INTERRUPT*: The thread is stopped in [zx_interrupt_wait](interrupt_wait.md). -->

* *ZX_THREAD_STATE_BLOCKED_EXCEPTION*：线程在异常中停止。
* *ZX_THREAD_STATE_BLOCKED_SLEEPING*：线程在[zx_nanosleep](nanosleep.md)调用中停止。
* *ZX_THREAD_STATE_BLOCKED_FUTEX*：线程在[zx_futex_wait](futex_wait.md)调用中停止。
* *ZX_THREAD_STATE_BLOCKED_PORT*：线程在[zx_port_wait](port_wait.md)调用中停止。
* *ZX_THREAD_STATE_BLOCKED_CHANNEL*：线程在[zx_channel_call](channel_call.md)调用中停止。
* *ZX_THREAD_STATE_BLOCKED_WAIT_ONE*：线程在[zx_object_wait_one](object_wait_one.md)调用中停止。
* *ZX_THREAD_STATE_BLOCKED_WAIT_MANY*：线程在[zx_object_wait_many](object_wait_many.md)调用中停止。
* *ZX_THREAD_STATE_BLOCKED_INTERRUPT*：线程在[zx_interrupt_wait](interrupt_wait.md)中调用停止。

<!-- The **ZX_EXCEPTION_PORT_TYPE_\*** values are defined by -->
**ZX_EXCEPTION_PORT_TYPE_\*** 值由以下头文件定义：
```
#include <zircon/syscalls/exception.h>
```

*   *ZX_EXCEPTION_PORT_TYPE_NONE*
*   *ZX_EXCEPTION_PORT_TYPE_DEBUGGER*
*   *ZX_EXCEPTION_PORT_TYPE_THREAD*
*   *ZX_EXCEPTION_PORT_TYPE_PROCESS*
*   *ZX_EXCEPTION_PORT_TYPE_JOB*

### ZX_INFO_THREAD_EXCEPTION_REPORT

<!-- *handle* type: **Thread** -->
*handle*类型：**线程** 

<!-- *buffer* type: **zx_exception_report_t[1]** -->
*buffer*类型：**zx_exception_report_t[1]**

```
#include <zircon/syscalls/exception.h>
```

<!-- If the thread is currently in an exception and is waiting for an exception
response, then this returns the exception report as a single
*zx_exception_report_t*, with status ZX_OK. -->
如果线程当前处于异常中并且正在等待异常响应，则调用会将异常报告作为单个*zx_exception_report_t*和状态*ZX_OK*一起返回。

<!-- Returns **ZX_ERR_BAD_STATE** if the thread is not in an exception and waiting for
an exception response. -->
如果线程不在异常中并等待异常响应，则返回**ZX_ERR_BAD_STATE**。

### ZX_INFO_THREAD_STATS

<!-- *handle* type: **Thread** -->
*handle*类型：**线程** 

<!-- *buffer* type: **zx_info_thread_stats[1]** -->
*buffer*类型：**zx_info_thread_stats[1]**

<!-- ```
typedef struct zx_info_thread_stats {
    // Total accumulated running time of the thread.
    zx_duration_t total_runtime;
} zx_info_thread_stats_t;
``` -->
```
typedef struct zx_info_thread_stats {
    // 线程累计总运行时间。
    zx_duration_t total_runtime;
} zx_info_thread_stats_t;
```

### ZX_INFO_CPU_STATS

<!-- Note: many values of this topic are being retired in favor of a different mechanism. -->
注意：因考虑使用不同的机制，该主题的许多值正在被弃用中。
<!-- *handle* type: **Resource** (Specifically, the root resource) -->
*handle*类型：**资源**（具体来讲是根资源）
<!-- *buffer* type: **zx_info_cpu_stats_t[1]** -->
*buffer*类型：**zx_info_cpu_stats_t[1]**
<!-- 
```
typedef struct zx_info_cpu_stats {
    uint32_t cpu_number;
    uint32_t flags;

    zx_duration_t idle_time;

    // kernel scheduler counters
    uint64_t reschedules;
    uint64_t context_switches;
    uint64_t irq_preempts;
    uint64_t preempts;
    uint64_t yields;

    // cpu level interrupts and exceptions
    uint64_t ints;          // hardware interrupts, minus timer interrupts
                            // inter-processor interrupts
    uint64_t timer_ints;    // timer interrupts
    uint64_t timers;        // timer callbacks
    uint64_t page_faults;   // (deprecated, returns 0)
    uint64_t exceptions;    // (deprecated, returns 0)
    uint64_t syscalls;

    // inter-processor interrupts
    uint64_t reschedule_ipis;
    uint64_t generic_ipis;
} zx_info_cpu_stats_t;
``` -->

```
typedef struct zx_info_cpu_stats {
    uint32_t cpu_number;
    uint32_t flags;

    zx_duration_t idle_time;

    // 内核调度程序计数器
    uint64_t reschedules;
    uint64_t context_switches;
    uint64_t irq_preempts;
    uint64_t preempts;
    uint64_t yields;

    // cpu级别的中断和异常
    uint64_t ints;          // 硬件中断次数，减去定时器中
                            // 断和处理器间中断次数
    uint64_t timer_ints;    // 定时器中断次数
    uint64_t timers;        // 定时器回调次数
    uint64_t page_faults;   // （已弃用，并返回0）
    uint64_t exceptions;    // （已弃用，并返回0）
    uint64_t syscalls;

    // 处理器间中断次数
    uint64_t reschedule_ipis;
    uint64_t generic_ipis;
} zx_info_cpu_stats_t;
```
### ZX_INFO_VMAR

<!-- *handle* type: **VM Address Region** -->
*handle*类型：**虚拟内存地址区域（VMAR）**

<!-- *buffer* type: **zx_info_vmar_t[1]** -->
*buffer*类型：**zx_info_vmar_t[1]**
<!-- 
```
typedef struct zx_info_vmar {
    // Base address of the region.
    uintptr_t base;

    // Length of the region, in bytes.
    size_t len;
} zx_info_vmar_t;
``` -->

```
typedef struct zx_info_vmar {
    // 区域的基地址
    uintptr_t base;

    // 区域的长度，以字节为单位
    size_t len;
} zx_info_vmar_t;
```

<!-- This returns a single *zx_info_vmar_t* that describes the range of address
space that the VMAR occupies. -->
调用返回单个*zx_info_vmar_t*类型，它描述了VMAR占用的地址空间的范围。

### ZX_INFO_VMO

<!-- *handle* type: **VM Object** -->
*handle*类型：**虚拟内存对象（VMO）**

<!-- *buffer* type: **zx_info_vmo_t[1]** -->
*buffer*类型：**zx_info_vmo_t[1]**
<!-- 
```
typedef struct zx_info_vmo {
    // The koid of this VMO.
    zx_koid_t koid;

    // The name of this VMO.
    char name[ZX_MAX_NAME_LEN];

    // The size of this VMO.
    uint64_t size_bytes;

    // If this VMO is a clone, the koid of its parent. Otherwise, zero.
    zx_koid_t parent_koid;

    // The number of clones of this VMO, if any.
    size_t num_children;

    // The number of times this VMO is currently mapped into VMARs.
    size_t num_mappings;

    // An estimate of the number of unique address spaces that
    // this VMO is mapped into.
    size_t share_count;

    // Bitwise OR of ZX_INFO_VMO_* values.
    uint32_t flags;

    // If |ZX_INFO_VMO_TYPE(flags) == ZX_INFO_VMO_TYPE_PAGED|, the amount of
    // memory currently allocated to this VMO.
    uint64_t committed_bytes;

    // If |flags & ZX_INFO_VMO_VIA_HANDLE|, the handle rights.
    // Undefined otherwise.
    zx_rights_t handle_rights;

    // VMO creation options. This is a bitmask of
    // kResizable    = (1u << 0);
    // kContiguous   = (1u << 1);
    uint32_t create_options;
} zx_info_vmo_t;
``` -->
```
// 描述VMO。
typedef struct zx_info_vmo {
    // 该VMO的koid值。
    zx_koid_t koid;

    // 该VMO的名称
    char name[ZX_MAX_NAME_LEN];

    // 该VMO的大小，即映射时它消耗的虚拟地址空间大小。
    uint64_t size_bytes;

    // 如果此VMO是其他某个VMO的副本，则为其父VMO的koid值，否则为零。
    // 关于副本的类型，参见|flags|。
    zx_koid_t parent_koid;

    // 该VMO的副本数（如果有的话）。
    size_t num_children;

    // 此VMO当前映射到VMAR的次数。 
    // 请注意，相同的进程通常会将同一个VMO映射两次，并且这两个映射都将计入此处。 （即，这不是映射此VMO的进程数的计数;请参阅share_count。）
    size_t num_mappings;

    // 估计此VMO映射到的唯一地址空间的数量。 
    // 每个进程都有自己的地址空间，内核也是如此。
    size_t share_count;

    // ZX_INFO_VMO_*的按位取或值。
    uint32_t flags;

    // 如果|ZX_INFO_VMO_TYPE(flags) == ZX_INFO_VMO_TYPE_PAGED|成立，该字段表示当前分配给此VMO的内存量，即它消耗的物理内存量，否则是未定义值。
    uint64_t committed_bytes;

    // 如果|flags & ZX_INFO_VMO_VIA_HANDLE|成立，该字段表示句柄的权限，否则是未定义值。
    zx_rights_t handle_rights;‘

    // VMO创建的标志位，是以下标识为的取或值：
    // kResizable    = (1u << 0);
    // kContiguous   = (1u << 1);
    uint32_t create_options;
} zx_info_vmo_t;
```

<!-- This returns a single *zx_info_vmo_t* that describes various attrubutes of
the VMO. -->
调用返回单个*zx_info_vmo_t*类型的值，它描述了VMO的各种属性。

### ZX_INFO_SOCKET

<!-- *handle* type: **Socket** -->
*handle*类型：**Socket**

<!-- *buffer* type: **zx_info_socket_t[1]** -->
*buffer*类型：**zx_info_socket_t[1]**

<!-- ```
typedef struct zx_info_socket {
    // The options passed to zx_socket_create().
    uint32_t options;

    // The value of ZX_PROP_SOCKET_RX_BUF_MAX.
    size_t rx_buf_max;

    // The value of ZX_PROP_SOCKET_RX_BUF_SIZE.
    size_t rx_buf_size;

    // The value of ZX_PROP_SOCKET_TX_BUF_MAX.
    //
    // Will be zero if the peer endpoint is closed.
    size_t tx_buf_max;

    // The value of ZX_PROP_SOCKET_TX_BUF_SIZE.
    //
    // Will be zero if the peer endpoint is closed.
    size_t tx_buf_size;
} zx_info_socket_t;
``` -->
```
typedef struct zx_info_socket {
    // 创建时传递给zx_socket_create()的选项。
    uint32_t options;

    // ZX_PROP_SOCKET_RX_BUF_MAX的值。
    size_t rx_buf_max;

    // ZX_PROP_SOCKET_RX_BUF_SIZE的值。
    size_t rx_buf_size;

    // ZX_PROP_SOCKET_TX_BUF_MAX的值。
    // 如果对等端点已关闭，则该字段为零。
    size_t tx_buf_max;

    // ZX_PROP_SOCKET_TX_BUF_SIZE的值。
    // 如果对等端点已关闭，则该字段为零。
    size_t tx_buf_size;
} zx_info_socket_t;
```

### ZX_INFO_JOB_CHILDREN

<!-- *handle* type: **Job** -->
*handle*类型：**作业**

<!-- *buffer* type: **zx_koid_t[n]** -->
*buffer*类型：**zx_koid_t[n]**
<!-- 
Returns an array of *zx_koid_t*, one for each direct child Job of the provided
Job handle. -->
返回*zx_koid_t*类型的数组，其中每一个对应于提供的作业句柄的其中一个子作业。

### ZX_INFO_JOB_PROCESSES

<!-- *handle* type: **Job** -->
*handle*类型：**作业**

<!-- *buffer* type: **zx_koid_t[n]** -->
*buffer*类型：**zx_koid_t[n]**

<!-- Returns an array of *zx_koid_t*, one for each direct child Process of the
provided Job handle. -->
返回*zx_koid_t*类型的数组，其中每一个对应于提供的作业句柄的其中一个子进程。

### ZX_INFO_TASK_STATS

<!-- *handle* type: **Process** -->
*handle*类型：**进程**

<!-- *buffer* type: **zx_info_task_stats_t[1]** -->
*buffer*类型：**zx_info_task_stats_t[1]**

<!-- Returns statistics about resources (e.g., memory) used by a task. -->
返回有关任务使用的资源（例如内存）的统计信息。

<!-- ```
typedef struct zx_info_task_stats {
    // The total size of mapped memory ranges in the task.
    // Not all will be backed by physical memory.
    size_t mem_mapped_bytes;

    // For the fields below, a byte is considered committed if it's backed by
    // physical memory. Some of the memory may be double-mapped, and thus
    // double-counted.

    // Committed memory that is only mapped into this task.
    size_t mem_private_bytes;

    // Committed memory that is mapped into this and at least one other task.
    size_t mem_shared_bytes;

    // A number that estimates the fraction of mem_shared_bytes that this
    // task is responsible for keeping alive.
    //
    // An estimate of:
    //   For each shared, committed byte:
    //   mem_scaled_shared_bytes += 1 / (number of tasks mapping this byte)
    //
    // This number is strictly smaller than mem_shared_bytes.
    size_t mem_scaled_shared_bytes;
} zx_info_task_stats_t;
``` -->
```
typedef struct zx_info_task_stats {
    // 任务中映射的内存范围的总量 
    // 但并非所有都将由物理内存提供。
    size_t mem_mapped_bytes;

    // 对于下面的字段，如果该字节由物理内存提供，则认为该字节已提交。
    // 其中部分内存可能会被重复映射，并产生重复计算。

    // 仅映射到此任务中已提交部分的内存。
    size_t mem_private_bytes;

    // 映射到此任务和至少一个其他任务的已提交内存。
    size_t mem_shared_bytes;

    // 用于估计此任务|mem_shared_bytes|内存中负责保持活跃的比例值
    //
    // 是以下值的一个估计：
    // 对于每个共享的，已提交的字节：
    // mem_scaled_shared_bytes + = 1 /(映射此字节的任务数量)
    //
    // 该数值会严格小于mem_shared_bytes。
    size_t mem_scaled_shared_bytes;
} zx_info_task_stats_t;
```

其他错误码：
<!-- *   **ZX_ERR_BAD_STATE**: If the target process is not currently running. -->
* **ZX_ERR_BAD_STATE**：目标进程当前未处于运行状态。

### ZX_INFO_PROCESS_MAPS

<!-- *handle* type: **Process** other than your own, with **ZX_RIGHT_READ** -->
*handle*类型：除了自身之外的其他带有**ZX_RIGHT_READ**权限的**进程**

<!-- *buffer* type: **zx_info_maps_t[n]** -->
*buffer*类型：**zx_info_maps_t[n]**

<!-- The *zx_info_maps_t* array is a depth-first pre-order walk of the target
process's Aspace/VMAR/Mapping tree. -->
*zx_info_maps_t*数组是目标进程的地址空间(ASpace)/VMAR/映射(Mapping)树的深度优先前序遍历数组。
 
<!-- ```
typedef struct zx_info_maps {
    // Name if available; empty string otherwise.
    char name[ZX_MAX_NAME_LEN];
    // Base address.
    zx_vaddr_t base;
    // Size in bytes.
    size_t size;

    // The depth of this node in the tree.
    // Can be used for indentation, or to rebuild the tree from an array
    // of zx_info_maps_t entries, which will be in depth-first pre-order.
    size_t depth;
    // The type of this entry; indicates which union entry is valid.
    uint32_t type; // zx_info_maps_type_t
    union {
        zx_info_maps_mapping_t mapping;
        // No additional fields for other types.
    } u;
} zx_info_maps_t;
``` -->
```
typedef struct zx_info_maps {
    // 名称（如果有的话），否则为空字符串
    char name[ZX_MAX_NAME_LEN];
    // 基地址
    zx_vaddr_t base;
    // 以字节为单位的映射总量
    size_t size;

    // 遍历树中此节点的深度。
    // 该字段可用于缩进，或从zx_info_maps_t类型的数组中以深度优先前序遍历重建出树
    size_t depth;
    // 该项的类型，用于表示联合体中的哪一项是有效的。
    uint32_t type; // zx_info_maps_type_t
    union {
        zx_info_maps_mapping_t mapping;
        // 对其他类型来讲，没有其他字段。
    } u;
} zx_info_maps_t;
```
<!-- 
The *depth* field of each entry describes its relationship to the nodes that
come before it. Depth 0 is the root Aspace, depth 1 is the root VMAR, and all
other entries have depth 2 or greater. -->
每一项的*depth*字段描述了它与前一节点之间的关系。 
其中深度为0的是根地址空间，深度为1的是根VMAR，所有其他项的深度至少为2。

<!-- To get a full picture of how a process uses its VMOs and how a VMO is used
by various processes, you may need to combine this information with
ZX_INFO_PROCESS_VMOS. -->
要全面了解进程如何使用其VMO以及VMO如何被各种进程使用，你需要将此信息与ZX_INFO_PROCESS_VMOS结合使用。

<!-- See the `vmaps` command-line tool for an example user of this topic, and to dump
the maps of arbitrary processes by koid. -->
有关此主题的示例用法，请参阅`vmaps`命令行工具，并通过koid转储任意进程的内存映射。

<!-- Additional errors: -->
其他错误码：

<!-- *   **ZX_ERR_ACCESS_DENIED**: If the appropriate rights are missing, or if a
    process attempts to call this on a handle to itself. It's not safe to
    examine yourself: *buffer* will live inside the Aspace being examined, and
    the kernel can't safely fault in pages of the buffer while walking the
    Aspace. -->
* **ZX_ERR_ACCESS_DENIED**：缺少适当的权限，或者进程试图在指向自己的句柄上调用此主题。 
  在自己的进程中调用本身是不安全的：*buffer*将存在于正在调用的地址空间中，并且内核在遍历地址空间时不能在缓冲区的页面中安全地触发错误。
<!-- *   **ZX_ERR_BAD_STATE**: If the target process is not currently running, or if
    its address space has been destroyed. -->
* **ZX_ERR_BAD_STATE**：目标进程当前未运行，或者其地址空间已被破坏。

### ZX_INFO_PROCESS_VMOS

<!-- 
*handle* type: **Process** other than your own, with **ZX_RIGHT_READ** -->
*handle*类型：除了自身之外的其他带有**ZX_RIGHT_READ**权限的**进程**

<!-- *buffer* type: **zx_info_vmos_t[n]** -->
*buffer*类型：**zx_info_vmos_t[n]**
<!-- 
The *zx_info_vmos_t* array is list of all VMOs pointed to by the target process.
Some VMOs are mapped, some are pointed to by handles, and some are both. -->
*zx_info_vmos_t*数组是目标进程指向的所有VMO的列表。 
其中部分VMO是已映射的，部分是由句柄所指向的，部分是两者皆存在。

<!-- **Note**: The same VMO may appear multiple times due to multiple
mappings/handles. Also, because VMOs can change as the target process runs,
the same VMO may have different values each time it appears. It is the
caller's job to resolve any duplicates. -->
**注意**：由于存在多个映射/句柄，同一个VMO可能会出现多次。 
此外，由于VMO可以在目标进程运行时改变，因此在VMO的不同时刻，相同的VMO可能具有不同的值。 
消解其中的任何重复项将留给调用者来处理。
<!-- 
To get a full picture of how a process uses its VMOs and how a VMO is used
by various processes, you may need to combine this information with
ZX_INFO_PROCESS_MAPS. -->
要全面了解进程如何使用其VMO以及VMO如何被多个进程使用，你需要将该内容与*ZX_INFO_PROCESS_MAPS*结合使用。
<!-- 
```
// Describes a VMO.
typedef struct zx_info_vmo {
    // The koid of this VMO.
    zx_koid_t koid;

    // The name of this VMO.
    char name[ZX_MAX_NAME_LEN];

    // The size of this VMO; i.e., the amount of virtual address space it
    // would consume if mapped.
    uint64_t size_bytes;

    // If this VMO is a clone, the koid of its parent. Otherwise, zero.
    // See |flags| for the type of clone.
    zx_koid_t parent_koid;

    // The number of clones of this VMO, if any.
    size_t num_children;

    // The number of times this VMO is currently mapped into VMARs.
    // Note that the same process will often map the same VMO twice,
    // and both mappings will be counted here. (I.e., this is not a count
    // of the number of processes that map this VMO; see share_count.)
    size_t num_mappings;

    // An estimate of the number of unique address spaces that
    // this VMO is mapped into. Every process has its own address space,
    // and so does the kernel.
    size_t share_count;

    // Bitwise OR of ZX_INFO_VMO_* values.
    uint32_t flags;

    // If |ZX_INFO_VMO_TYPE(flags) == ZX_INFO_VMO_TYPE_PAGED|, the amount of
    // memory currently allocated to this VMO; i.e., the amount of physical
    // memory it consumes. Undefined otherwise.
    uint64_t committed_bytes;

    // If |flags & ZX_INFO_VMO_VIA_HANDLE|, the handle rights.
    // Undefined otherwise.
    zx_rights_t handle_rights;
} zx_info_vmo_t;
``` -->

```
// 描述VMO。
typedef struct zx_info_vmo {
    // 该VMO的koid值。
    zx_koid_t koid;

    // 该VMO的名称
    char name[ZX_MAX_NAME_LEN];

    // 该VMO的大小，即映射时它消耗的虚拟地址空间大小。
    uint64_t size_bytes;

    // 如果此VMO是其他某个VMO的副本，则为其父VMO的koid值，否则为零。
    // 关于副本的类型，参见|flags|。
    zx_koid_t parent_koid;

    // 该VMO的副本数（如果有的话）。
    size_t num_children;

    // 此VMO当前映射到VMAR的次数。 
    // 请注意，相同的进程通常会将同一个VMO映射两次，并且这两个映射都将计入此处。 （即，这不是映射此VMO的进程数的计数;请参阅share_count。）
    size_t num_mappings;

    // 估计此VMO映射到的唯一地址空间的数量。 
    // 每个进程都有自己的地址空间，内核也是如此。
    size_t share_count;

    // ZX_INFO_VMO_*的按位取或值。
    uint32_t flags;

    // 如果|ZX_INFO_VMO_TYPE(flags) == ZX_INFO_VMO_TYPE_PAGED|成立，该字段表示当前分配给此VMO的内存量，即它消耗的物理内存量，否则是未定义值。
    uint64_t committed_bytes;

    // 如果|flags & ZX_INFO_VMO_VIA_HANDLE|成立，该字段表示句柄的权限，否则是未定义值。
    zx_rights_t handle_rights;
} zx_info_vmo_t;
```

<!-- See the `vmos` command-line tool for an example user of this topic, and to dump
the VMOs of arbitrary processes by koid. -->
有关此主题的示例用法，请参阅`vmos`命令行工具，并通过koid转储任意进程的VMO。

### ZX_INFO_KMEM_STATS

<!-- *handle* type: **Resource** (Specifically, the root resource) -->
*handle*类型：**资源**（具体来讲是根资源）

<!-- *buffer* type: **zx_info_kmem_stats_t[1]** -->
*buffer*类型：**zx_info_kmem_stats_t[1]**

<!-- Returns information about kernel memory usage. It can be expensive to gather. -->
返回有关内核内存使用情况的信息。
但是收集这些信息可能开销很大。

<!-- ```
typedef struct zx_info_kmem_stats {
    // The total amount of physical memory available to the system.
    size_t total_bytes;

    // The amount of unallocated memory.
    size_t free_bytes;

    // The amount of memory reserved by and mapped into the kernel for reasons
    // not covered by other fields in this struct. Typically for readonly data
    // like the ram disk and kernel image, and for early-boot dynamic memory.
    size_t wired_bytes;

    // The amount of memory allocated to the kernel heap.
    size_t total_heap_bytes;

    // The portion of |total_heap_bytes| that is not in use.
    size_t free_heap_bytes;

    // The amount of memory committed to VMOs, both kernel and user.
    // A superset of all userspace memory.
    // Does not include certain VMOs that fall under |wired_bytes|.
    //
    // TODO(dbort): Break this into at least two pieces: userspace VMOs that
    // have koids, and kernel VMOs that don't. Or maybe look at VMOs
    // mapped into the kernel aspace vs. everything else.
    size_t vmo_bytes;

    // The amount of memory used for architecture-specific MMU metadata
    // like page tables.
    size_t mmu_overhead_bytes;

    // Non-free memory that isn't accounted for in any other field.
    size_t other_bytes;
} zx_info_kmem_stats_t;
``` -->
```
typedef struct zx_info_kmem_stats {
    // 系统可用的物理内存总量。
    size_t total_bytes;

    // 未分配的内存总量。
    size_t free_bytes;

    // 保留并映射到内核，但此结构体中其他字段未涵盖的内存量。 
    // 这部分内存通常用于读取磁盘和内核映像等只读数据，以及早期启动的动态内存。
    size_t wired_bytes;

    // 分配给内核堆空间的内存量。
    size_t total_heap_bytes;

    // |total_heap_bytes|中的未使用部分。
    size_t free_heap_bytes;

    // 提交给VMO（内核态和用户空间部分）的内存量。
    // 所有用户空间内存的超集，但不包括某些属于|wired_bytes|的VMO。
    // TODO(dbort)：将其分为至少两部分：具有koid值的用户空间VMO和不具有koid的内核态VMO，或者可以考虑将VMO映射到内核空间/其他位置。
    size_t vmo_bytes;

    // 用于特定于体系结构的MMU元数据（如页表）的内存量。
    size_t mmu_overhead_bytes;

    // 在任何其他字段中未包含在内的非空闲内存。
    size_t other_bytes;
} zx_info_kmem_stats_t;
```

### ZX_INFO_RESOURCE

<!-- *handle* type: **Resource**
*buffer* type: **zx_info_resource_t[1]** -->
*handle*类型：**资源**

*buffer*类型：**zx_info_resource_t[1]**

<!-- Returns information about a resource object via its handle. -->
通过其句柄返回有关资源对象的信息。

<!-- 
```
typedef struct zx_info_resource {
    // The resource kind
    uint32_t kind;
    // Resource's low value (inclusive)
    uint64_t low;
    // Resource's high value (inclusive)
    uint64_t high;
} zx_info_resource_t;
``` -->
```
typedef struct zx_info_resource {
    // 资源种类
    uint32_t kind;
    // 资源的最低值（包括）
    uint64_t low;
    // 资源的最高值（包括）
    uint64_t high;
} zx_info_resource_t;
```

资源类型为下列其中之一：

*   *ZX_RSRC_KIND_ROOT*
*   *ZX_RSRC_KIND_MMIO*
*   *ZX_RSRC_KIND_IOPORT*
*   *ZX_RSRC_KIND_IRQ*
*   *ZX_RSRC_KIND_HYPERVISOR*

### ZX_INFO_BTI

<!-- *handle* type: **Bus Transaction Initiator** -->
*handle*类型：**总线事务启动器（BTI）**
<!-- *buffer* type: **zx_info_bti_t[1]** -->
*buffer*类型：**zx_info_bti_t[1]**

<!-- ```
typedef struct zx_info_bti {
    // zx_bti_pin will always be able to return addreses that are contiguous for at
    // least this many bytes.  E.g. if this returns 1MB, then a call to
    // zx_bti_pin() with a size of 2MB will return at most two physically-contiguous runs.
    // If the size were 2.5MB, it will return at most three physically-contiguous runs.
    uint64_t minimum_contiguity;

    // The number of bytes in the device's address space (UINT64_MAX if 2^64).
    uint64_t aspace_size;
} zx_info_bti_t;
``` -->
```
typedef struct zx_info_bti {
    // zx_bti_pin将始终能够返回至少这么多字节的连续地址。 
    // 例如，如果该字段返回1MB，则使用大小为2MB的VMO调用zx_bti_pin()将最多返回两个连续物理的空间。 
    // 如果使用大小为2.5MB的VMO，它将最多返回三个连续物理的空间。
    uint64_t minimum_contiguity;

    // 设备地址空间中的字节数（如果字段为2^64，则为UINT64_MAX）。
    uint64_t aspace_size;
} zx_info_bti_t;
```

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_object_get_info**() returns **ZX_OK** on success. In the event of
failure, a negative error value is returned. -->
**zx_object_get_info()** 执行成功则返回**ZX_OK**。
如果发生错误，将返回以下错误码之一。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE** *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE** *handle* is not an appropriate type for *topic* -->
**ZX_ERR_WRONG_TYPE**：*handle*不符合*topic*所需要的类型

<!-- **ZX_ERR_ACCESS_DENIED**: If *handle* does not have the necessary rights for the
operation. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有需要的操作权限。

<!-- **ZX_ERR_INVALID_ARGS** *buffer*, *actual*, or *avail* are invalid pointers. -->
**ZX_ERR_INVALID_ARGS**：*buffer*，*actual*或*avail*是无效指针。

<!-- 
**ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_BUFFER_TOO_SMALL** The *topic* returns a fixed number of records, but the
provided buffer is not large enough for these records. -->
**ZX_ERR_BUFFER_TOO_SMALL**：该*topic*返回固定数量的记录，但提供的缓冲区不足以存放这些记录。

<!-- **ZX_ERR_NOT_SUPPORTED** *topic* does not exist. -->
**ZX_ERR_NOT_SUPPORTED**：*topic*主题不存在。

<!-- ## EXAMPLES -->
## 示例

```
bool is_handle_valid(zx_handle_t handle) {
    return zx_object_get_info(
        handle, ZX_INFO_HANDLE_VALID, NULL, 0, NULL, NULL) == ZX_OK;
}

zx_koid_t get_object_koid(zx_handle_t handle) {
    zx_info_handle_basic_t info;
    if (zx_object_get_info(handle, ZX_INFO_HANDLE_BASIC,
                           &info, sizeof(info), NULL, NULL) != ZX_OK) {
        return 0;
    }
    return info.koid;
}

void examine_threads(zx_handle_t proc) {
    zx_koid_t threads[128];
    size_t count, avail;

    if (zx_object_get_info(proc, ZX_INFO_PROCESS_THREADS, threads,
                           sizeof(threads), &count, &avail) != ZX_OK) {
        // Error!
    } else {
        if (avail > count) {
            // More threads than space in array;
            // could call again with larger array.
        }
        for (size_t n = 0; n < count; n++) {
            do_something(thread[n]);
        }
    }
}
```

<!-- ## SEE ALSO -->
## 另见

[handle_close](handle_close.md), [handle_duplicate](handle_duplicate.md),
[handle_replace](handle_replace.md), [object_get_child](object_get_child.md).

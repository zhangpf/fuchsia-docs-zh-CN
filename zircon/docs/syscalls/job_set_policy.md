# zx_job_set_policy
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/a73de2ea77ae17a6b2e57286299e57cc2b838dc7/docs/syscalls/job_set_policy.md)

---
<!-- ## NAME -->
## 名称

<!-- job_set_policy - Set job security and resource policies. -->
job_set_policy —— 设置作业的安全和资源策略。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/policy.h>

zx_status_t zx_job_set_policy(zx_handle_t job_handle, uint32_t options,
                              uint32_t topic, const void* policy, uint32_t count);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- Sets one or more security and/or resource policies to an empty job. The job's
effective policies is the combination of the parent's effective policies and
the policies specified in *policy*. The effect in the case of conflict between
the existing policies and the new policies is controlled by *options* values: -->
为空作业设置一个或多个安全和/或资源策略。 
该作业的有效策略是父作业的有效策略与*policy*中指定的策略的组合。 
现有策略策与新策略之间发生冲突时产生的影响由*options*值所控制：

<!-- + **ZX_JOB_POL_RELATIVE** : policy is applied for the conditions not specifically
  overridden by the parent policy.
+ **ZX_JOB_POL_ABSOLUTE** : policy is applied for all conditions in *policy* or
  the syscall fails. -->
+ **ZX_JOB_POL_RELATIVE**：未由父策略明确覆盖的条件所指定的策略被实施。
+ **ZX_JOB_POL_ABSOLUTE**：要么*policy*中的所有条件被实施，要么此系统调用失败。
<!-- After this call succeeds any new child process or child job will have the new
effective policy applied to it. -->
在此系统调用成功后，任何新的子进程或子作业都将应用这个新有效策略。

<!-- *topic* indicates the *policy* format. Supported value is **ZX_JOB_POL_BASIC**
which indicates that *policy* is an array of *count* entries of: -->

*topic*指定了*policy*的格式。它支持的值仅是**ZX_JOB_POL_BASIC**，表示*policy*是*count*个如下类型的项的数组：

```
typedef struct zx_policy_basic {
    uint32_t condition;
    uint32_t policy;
} zx_policy_basic_t;

```

<!-- Where *condition* is one of -->
其中*condition*是下列值之一：
<!-- + **ZX_POL_BAD_HANDLE** a process under this job is attempting to
  issue a syscall with an invalid handle.  In this case,
  **ZX_POL_ACTION_ALLOW** and **ZX_POL_ACTION_DENY** are equivalent:
  if the syscall returns, it will always return the error
  **ZX_ERR_BAD_HANDLE**. -->
+ **ZX_POL_BAD_HANDLE**：此作业下的进程尝试发起带有无效句柄的系统调用。
  在这种情况下，**ZX_POL_ACTION_ALLOW**和**ZX_POL_ACTION_DENY**是等效的：如果系统调用返回，它将始终返回**ZX_ERR_BAD_HANDLE**错误。
<!-- + **ZX_POL_WRONG_OBJECT** a process under this job is attempting to
  issue a syscall with a handle that does not support such operation. -->
+ **ZX_POL_WRONG_OBJECT**：此作业下的进程尝试使用不支持此类操作的句柄发起系统调用。
<!-- + **ZX_POL_VMAR_WX** a process under this job is attempting to map an
  address region with write-execute access. -->
+ **ZX_POL_VMAR_WX**：此作业下的进程尝试使用写入+执行的权限来进行地址区域的映射。
<!-- + **ZX_POL_NEW_VMO** a process under this job is attempting to create
  a new vm object. -->
+ **ZX_POL_NEW_VMO**：此作业下的进程尝试创建新的vm对象。
<!-- + **ZX_POL_NEW_CHANNEL** a process under this job is attempting to create
  a new channel. -->
+ **ZX_POL_NEW_CHANNEL**：此作业下的进程尝试创建新的channel对象。
<!-- + **ZX_POL_NEW_EVENT** a process under this job is attempting to create
  a new event. -->
+ **ZX_POL_NEW_EVENT**：此作业下的进程尝试创建新的event对象。
<!-- + **ZX_POL_NEW_EVENTPAIR** a process under this job is attempting to create
  a new event pair. -->
+ **ZX_POL_NEW_EVENTPAIR**：此作业下的进程程尝试创建新的eventpair对象。
<!-- + **ZX_POL_NEW_PORT** a process under this job is attempting to create
  a new port. -->
+ **ZX_POL_NEW_PORT**：此作业下的进程程尝试创建新的port对象。
<!-- + **ZX_POL_NEW_SOCKET** a process under this job is attempting to create
  a new socket. -->
+ **ZX_POL_NEW_SOCKET**：此作业下的进程程尝试创建新的socket对象。

<!-- + **ZX_POL_NEW_FIFO** a process under this job is attempting to create
  a new fifo. -->
+ **ZX_POL_NEW_FIFO**：此作业下的进程程尝试创建新的fifo对象。

<!-- + **ZX_POL_NEW_TIMER** a process under this job is attempting to create
  a new timer. -->
+ **ZX_POL_NEW_TIMER**：此作业下的进程程尝试创建新的timer对象。
<!-- + **ZX_POL_NEW_PROCESS** a process under this job is attempting to create
  a new process. -->
+ **ZX_POL_NEW_PROCESS**：此作业下的进程程尝试创建新的process对象。

<!-- + **ZX_POL_NEW_ANY** is a special *condition* that stands for all of
  the above **ZX_NEW** condtions such as **ZX_POL_NEW_VMO**,
  **ZX_POL_NEW_CHANNEL**, **ZX_POL_NEW_EVENT**, **ZX_POL_NEW_EVENTPAIR**,
  **ZX_POL_NEW_PORT**, **ZX_POL_NEW_SOCKET**, **ZX_POL_NEW_FIFO**,
  and any future ZX_NEW policy. This will include any new
  kernel objects which do not require a parent object for creation. -->
+ **ZX_POL_NEW_ANY**是一个特殊*condition*值，代表所有上述**ZX_NEW**条件值，例如**ZX_POL_NEW_VMO**，**ZX_POL_NEW_CHANNEL**，**ZX_POL_NEW_EVENT**，**ZX_POL_NEW_EVENTPAIR**，**ZX_POL_NEW_PORT**，**ZX_POL_NEW_SOCKET**，**ZX_POL_NEW_FIFO**，以及任何未来的新的*ZX_NEW*策略。 
  它也包括不需要父对象来创建的任何新内核对象。

<!-- Where *policy* is either
+ **ZX_POL_ACTION_ALLOW**  allow *condition*.
+ **ZX_POL_ACTION_DENY**  prevent *condition*. -->
其中*policy*是下列值之一：
+ **ZX_POL_ACTION_ALLOW**：允许*condition*。
+ **ZX_POL_ACTION_DENY**：阻止*condition*。
<!-- 
Optionally it can be augmented via OR with
+ **ZX_POL_ACTION_EXCEPTION** generate an exception via the debug port. An
  exception generated this way acts as a breakpoint. The thread may be
  resumed after the exception.
+ **ZX_POL_ACTION_KILL** terminate the process. It also
implies **ZX_POL_ACTION_DENY**. -->
作为选项，*policy*可以通过OR操作来增强：
+ **ZX_POL_ACTION_EXCEPTION**：通过调试端口产生异常。 
  以这种方式生成的异常充当断点，并且异常发送后可以恢复该线程。
+ **ZX_POL_ACTION_KILL**：终止进程，这也同时蕴含着**ZX_POL_ACTION_DENY **。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_job_set_policy**() returns **ZX_OK** on success.  In the event of failure,
a negative error value is returned. -->
**zx_job_set_policy()** 调用成功返回**ZX_OK**。如果调用失败，则返回负的错误码。

<!-- ## NOTES -->
## 注意
<!-- 
The **ZX_POL_BAD_HANDLE** policy does not apply when calling ``zx_object_get_info()``
with the topic ZX_INFO_HANDLE_VALID.  All other topics and all other syscalls that
take handles are subject to the policy. -->
当使用**ZX_INFO_HANDLE_VALID**主题调用``zx_object_get_info()``时，**ZX_POL_BAD_HANDLE**策略不会实施。 
所有其他主题和所有其他系统调用都受到策略的限制。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_INVALID_ARGS**  *policy* was not a valid pointer, or *count* was 0,
or *policy* was not **ZX_JOB_POL_RELATIVE** or **ZX_JOB_POL_ABSOLUTE**, or
*topic* was not **ZX_JOB_POL_BASIC**. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*policy*是无效指针；*count*是0；*policy*不是**ZX_JOB_POL_RELATIVE**或**ZX_JOB_POL_ABSOLUTE**；*topic*不是**ZX_JOB_POL_BASIC**。

<!-- **ZX_ERR_BAD_HANDLE**  *job_handle* is not valid handle. -->
**ZX_ERR_BAD_HANDLE**：*job_handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *job_handle* is not a job handle. -->
**ZX_ERR_WRONG_TYPE**：*job_handle*不是作业类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *job_handle* does not have ZX_POL_RIGHT_SET right. -->
**ZX_ERR_ACCESS_DENIED**：*job_handle*没有*ZX_POL_RIGHT_SET*权限。

<!-- **ZX_ERR_BAD_STATE**  the job has existing jobs or processes alive. -->
**ZX_ERR_BAD_STATE**：作业已有子作业或进程处于存活状态。

<!-- **ZX_ERR_OUT_OF_RANGE** *count* is bigger than ZX_POL_MAX or *condition* is
bigger than ZX_POL_MAX. -->
**ZX_ERR_OUT_OF_RANGE**：*count*大于*ZX_POL_MAX*或*condition*大于*ZX_POL_MAX*。

<!-- **ZX_ERR_ALREADY_EXISTS** existing policy conflicts with the new policy. -->
**ZX_ERR_ALREADY_EXISTS**：现有策略与新设置的策略冲突。

<!-- **ZX_ERR_NOT_SUPPORTED** an entry in *policy* has an invalid value. -->
**ZX_ERR_NOT_SUPPORTED**：*policy*中的项具有无效值。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[job_create](job_create.md).
[process_create](job_create.md).
[object_get_info](object_get_info.md).
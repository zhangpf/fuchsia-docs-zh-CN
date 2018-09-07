# zx_job_create
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/a73de2ea77ae17a6b2e57286299e57cc2b838dc7/docs/syscalls/job_create.md)

---
<!-- ## NAME -->
## 名称

<!-- job_create - create a new job -->
job_create —— 创建新的子作业

<!-- ## SYNOPSIS -->
## 概要
```
#include <zircon/syscalls.h>

zx_status_t zx_job_create(zx_handle_t job, uint32_t options, zx_handle_t* out);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **job_create**() creates a new child [job object](../objects/job.md) given a
parent job. -->
**job_create()** 在给定父作业的情况下创建一个新的子[作业对象](../objects/job.md)。

<!-- Upon success a handle for the new job is returned. -->
如果调用成功，则返回新创建的作业的句柄。

<!-- The kernel keeps track of and restricts the "height" of a job, which is its
distance from the root job. It is illegal to create a job under a parent whose
height exceeds an internal "max height" value. (It is, however, legal to create
a process under such a job.) -->
内核跟踪并限制作业的“高度值”，即与根作业的距离值。 
在高度超过内部“最大高度值”的父作业下创建子作业是非法的（但在该作业下创建进程却是合法的）。

<!-- Job handles may be waited on (TODO(cpu): expand this) -->
作业句柄可以被用于等待（TODO(cpu)：展开描述）。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **job_create**() returns ZX_OK and a handle to the new job
(via *out*) on success.  In the event of failure, a negative error value
is returned. -->
**job_create()** 调用成功则返回*ZX_OK*和（通过*out*）返回新作业的句柄，如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *job* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*job*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *job* is not a job handle. -->
**ZX_ERR_WRONG_TYPE**：*job*不是作业类型的句柄。

<!-- **ZX_ERR_INVALID_ARGS**  *options* is nonzero, or *out* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*options*是非零值，或*out*是无效指针。

<!-- **ZX_ERR_ACCESS_DENIED**  *job* does not have the **ZX_RIGHT_WRITE** or **ZX_RIGHT_MANAGE_JOB** right. -->
**ZX_ERR_ACCESS_DENIED**：*job*没有**ZX_RIGHT_WRITE**或**ZX_RIGHT_MANAGE_JOB**权限。

<!-- **ZX_ERR_OUT_OF_RANGE**  The height of *job* is too large to create a child job. -->
**ZX ERR_OUT_OF_RANGE**：*job*的高度值太大，无法创建子作业。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_BAD_STATE**  The parent job object is in the dead state. -->
**ZX_ERR_BAD_STATE**：父作业对象处于已死亡状态。

<!-- ## SEE ALSO -->
## 另见

[process_create](process_create.md), [task_kill](task_kill.md), [object_get_property](object_get_property.md).
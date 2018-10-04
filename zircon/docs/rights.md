<!-- # Rights -->
# 权限(Right)
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/599a37ad75ad6a3e7963a808e9f05c8fd6865749/docs/rights.md)

---
<!-- ## Basics -->
## 基础

<!-- Rights are associated with handles and convey privileges to perform actions on
either the associated handle or the object associated with the handle. -->
权限与句柄相关联，并用于表达对关联句柄或与句柄关联的对象执行操作的所需特权。

<!-- The [`<zircon/rights.h>`](../system/public/zircon/rights.h) header defines
default rights for each object type, which can be reduced via
`zx_handle_replace()` or `zx_handle_duplicate()`. -->
[`<zircon/rights.h>`](https://github.com/fuchsia-mirror/zircon/blob/master/system/public/zircon/rights.h)头文件定义了每种对象类型的默认权限，并可以通过`zx_handle_replace()`或`zx_handle_duplicate()`来减少权限。

<!-- | Right | Conferred Privileges |
| ----- | -------------------- |
| **ZX_RIGHT_DUPLICATE**      | Allows handle duplication via [*zx_handle_duplicate*](syscalls/handle_duplicate.md) |
| **ZX_RIGHT_TRANSFER**       | Allows handle transfer via [*zx_channel_write*](syscalls/channel_write.md) |
| **ZX_RIGHT_READ**           | **TO BE REMOVED** Allows inspection of object state |
|                             | Allows reading of data from containers (channels, sockets, VM objects, etc) |
|                             | Allows mapping as readable if **ZX_RIGHT_MAP** is also present |
| **ZX_RIGHT_WRITE**          | **TO BE REMOVED** Allows modification of object state |
|                             | Allows writing of data to containers (channels, sockets, VM objects, etc) |
|                             | Allows mapping as writeable if **ZX_RIGHT_MAP** is also present |
| **ZX_RIGHT_EXECUTE**        | Allows mapping as executable if **ZX_RIGHT_MAP** is also present |
| **ZX_RIGHT_MAP**            | Allows mapping of a VM object into an address space. |
| **ZX_RIGHT_GET_PROPERTY**   | Allows property inspection via [*zx_object_get_property*](syscalls/object_get_property.md) |
| **ZX_RIGHT_SET_PROPERTY**   | Allows property modification via [*zx_object_set_property*](syscalls/object_set_property.md) |
| **ZX_RIGHT_ENUMERATE**      | Allows enumerating child objects via [*zx_object_get_info*](syscalls/object_get_info.md) and [*zx_object_get_child*](syscalls/object_get_child.md) |
| **ZX_RIGHT_DESTROY**        | Allows termination of task objects via [*zx_task_kill*](syscalls/task_kill.md)|
| **ZX_RIGHT_SET_POLICY**     | Allows policy modification via [*zx_job_set_policy*](syscalls/job_set_policy.md)|
| **ZX_RIGHT_GET_POLICY**     | Allows policy inspection via [*zx_job_get_policy*](syscalls/job_get_policy.md)|
| **ZX_RIGHT_SIGNAL**         | Allows use of [*zx_object_signal*](syscalls/object_signal.md) |
| **ZX_RIGHT_SIGNAL_PEER**    | Allows use of [*zx_object_signal_peer*](syscalls/object_signal.md) |
| **ZX_RIGHT_WAIT**           | Allows use of [*zx_object_wait_one*](syscalls/object_wait_one.md), [*zx_object_wait_many*](syscalls/object_wait_many.md), and other waiting primitives |
| **ZX_RIGHT_INSPECT**        | Allows inspection via [*zx_object_get_info*](syscalls/object_get_info.md) |
| **ZX_RIGHT_MANAGE_JOB**     | **NOT YET IMPLEMENTED** Allows creation of processes, subjobs, etc. |
| **ZX_RIGHT_MANAGE_PROCESS** | **NOT YET IMPLEMENTED** Allows creation of threads, etc |
| **ZX_RIGHT_MANAGE_THREAD**  | **NOT YET IMPLEMENTED** Allows suspending/resuming threads, etc| -->

| 权限 | 授予的特权 |
| ----- | -------------------- |
| **ZX_RIGHT_DUPLICATE**      | 允许通过[*zx_handle_duplicate*](syscalls/handle_duplicate.md)进行句柄复制 |
| **ZX_RIGHT_TRANSFER**       | 允许通过[*zx_channel_write*](syscalls/channel_write.md)进行句柄传输 |
| **ZX_RIGHT_READ**           | **即将被删除** 允许读取对象的状态 |
|                             | 允许从容器（通道，Socket和VMO等）中读取数据 |
|                             | 如果存在权限**ZX_RIGHT_MAP**，则允许内存映射为可读 |
| **ZX_RIGHT_WRITE**          | **即将被删除** 允许修改对象的状态 |
|                             | 允许将数据写入容器（通道，Socket和VMO等） |
|                             | 如果存在权限**ZX_RIGHT_MAP**，则允许内存映射为可写 |
| **ZX_RIGHT_EXECUTE**        | 如果存在权限**ZX_RIGHT_MAP**，则允许内存映射为可执行 |
| **ZX_RIGHT_MAP**            | 允许将虚拟内存对象映射到地址空间 |
| **ZX_RIGHT_GET_PROPERTY**   | 允许通过[*zx_object_get_property*](syscalls/object_get_property.md)获取对象属性 |
| **ZX_RIGHT_SET_PROPERTY**   | 允许通过[*zx_object_set_property*](syscalls/object_set_property.md)修改对象属性 |
| **ZX_RIGHT_ENUMERATE**      | 允许通过[*zx_object_get_info*](syscalls/object_get_info.md)和[*zx_object_get_child*](syscalls/object_get_child.md)枚举子对象 |
| **ZX_RIGHT_DESTROY**        | 允许通过[*zx_task_kill*](syscalls/task_kill.md)终止任务对象 |
| **ZX_RIGHT_SET_POLICY**     | 允许通过[*zx_job_set_policy*](syscalls/job_set_policy.md)修改策略 |
| **ZX_RIGHT_GET_POLICY**     | 允许通过[*zx_job_get_policy*](syscalls/job_get_policy.md)获取策略 |
| **ZX_RIGHT_SIGNAL**         | 允许使用[*zx_object_signal*](syscalls/object_signal.md) |
| **ZX_RIGHT_SIGNAL_PEER**    | 允许使用[*zx_object_signal_peer*](syscalls/object_signal_peer.md) |
| **ZX_RIGHT_WAIT**           | 允许使用[*zx_object_wait_one*](syscalls/object_wait_one.md)和[*zx_object_wait_many*](syscalls/object_wait_many.md)，以及其他等待原语 |
| **ZX_RIGHT_INSPECT**        | 允许使用[*zx_object_get_info*](syscalls/object_get_info.md)读取 |
| **ZX_RIGHT_MANAGE_JOB**     | **尚未实现** 允许创建进程和子作业等。 |
| **ZX_RIGHT_MANAGE_PROCESS** | **尚未实现** 允许创建线程等 |
| **ZX_RIGHT_MANAGE_THREAD**  | **尚未实现** 允许挂起/恢复线程等 |


<!-- ## See also -->
## 另见

[Objects](objects.md),
[Handles](handles.md)
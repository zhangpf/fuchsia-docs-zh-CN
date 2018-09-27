# zx_object_get_property, zx_object_set_property
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ebf318d4c9c0b4f64709d0a978c019129a49cfc/docs/syscalls/object_get_property.md)

---
<!-- ## NAME -->
## 名称
<!-- 
object_get_property - Ask for various properties of various kernel objects. -->
object_get_property —— 获取内核对象的各种属性值。

<!-- object_set_property - Set various properties of various kernel objects. -->
object_set_property —— 设置内核对象的各种属性值。

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/object.h>

zx_status_t zx_object_get_property(zx_handle_t handle, uint32_t property,
                                   void* value, size_t value_size);

zx_status_t zx_object_set_property(zx_handle_t handle, uint32_t property,
                                   const void* value, size_t value_size);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_object_get_property()** requests the value of a kernel object's property.
Getting a property requires **ZX_RIGHT_GET_PROPERTY** rights on the handle. -->
**zx_object_get_property()** 的功能是获取内核对象的属性值。 
获取属性需要句柄具有**ZX_RIGHT_GET_PROPERTY**权限。

<!-- **zx_object_set_property()** modifies the value of a kernel object's property.
Setting a property requires **ZX_RIGHT_SET_PROPERTY** rights on the handle. -->
**zx_object_set_property()** 的功能是修改内核对象的属性值。
修改属性需要句柄具有**ZX_RIGHT_SET_PROPERTY**权限。

<!-- The *handle* parameter indicates the target kernel object. Different properties
only work on certain types of kernel objects, as described below. -->
*handle*参数用于表示目标内核对象。 
不同的属性仅适用于特定类型的内核对象，具体如下所述。

<!-- The *property* parameter indicates which property to get/set. Property values
have the prefix **ZX_PROP_**, and are described below. -->
*property*参数用于表示要获取/设置的属性。
属性值带有前缀**ZX_PROP_**，具体如下所述。

<!-- The *value* parameter holds the property value, and must be a pointer to a
buffer of *size* bytes. Different properties expect different value types/sizes
as described below. -->
*value*参数用于保存属性值，并且必须是指向*size*字节大小的缓冲区的指针。 
不同的属性需要不同类型/大小的值，具体如下所述。

<!-- ## PROPERTIES -->
## 属性

<!-- Property values have the prefix **ZX_PROP_**, and are defined in -->
属性值都带有**ZX_PROP_** 前缀，并在下面文件中定义

```
#include <zircon/syscalls/object.h>
```

### ZX_PROP_NAME

<!-- *handle* type: **(Most types)** -->
*handle*类型：**（大多数的类型）**

<!-- *value* type: **char\[ZX_MAX_NAME_LEN\]** -->
 *value*类型：**char\[ZX_MAX_NAME_LEN\]**

<!-- Allowed operations: **get**, **set** -->
允许操作：**get**，**set**
<!-- The name of the object, as a NUL-terminated string. -->
对象的名称，以`NULL`结尾的字符串。

<!-- ### ZX_PROP_REGISTER_FS and ZX_PROP_REGISTER_GS -->
### ZX_PROP_REGISTER_FS和ZX_PROP_REGISTER_GS

<!-- *handle* type: **Thread** -->
*handle*类型：**Thread**

<!-- *value* type: **uintptr_t** -->
*value*类型：**uintptr_t**

<!-- Allowed operations: **set** -->
允许操作：**set**

<!-- The value of the x86 FS or GS segment register. `value` must be a
canonical address, and must be a userspace address. -->
x86处理器的FS或GS段寄存器的值。
`value`必须是规范的地址，并且必须是用户空间中的地址。

<!-- Only defined for x86-64. -->
仅为x86-64体系结构所定义。

### ZX_PROP_PROCESS_DEBUG_ADDR

<!-- *handle* type: **Process** -->
*handle*类型：**Process**

<!-- *value* type: **uintptr_t** -->
*value*类型：**uintptr_t**

<!-- Allowed operations: **get**, **set** -->
允许操作：**get**，**set**

<!-- The value of ld.so's `_dl_debug_addr`. This can be used by debuggers to
interrogate the state of the dynamic loader. -->
ld.so的值是`_dl_debug_addr`。 
调试器可以使用它来查询动态加载器的状态。

<!-- If this value is set to `ZX_PROCESS_DEBUG_ADDR_BREAK_ON_SET` on process
creation, the loader will manually issue a debug breakpoint when the property
has been set to its correct value. This gives an opportunity to read or modify
the initial state of the program. -->
如果在创建进程时将此值设置为`ZX_PROCESS_DEBUG_ADDR_BREAK_ON_SET`，接下来的运行中，在将该属性设置为正确的值时，加载程序将手动地发出调试断点中断。
这为读取或修改程序初始状态提供了手段。

### ZX_PROP_PROCESS_VDSO_BASE_ADDRESS

<!-- *handle* type: **Process** -->
*handle*类型：**Process**

<!-- *value* type: **uintptr_t** -->
*value*类型：**uintptr_t**

<!-- Allowed operations: **get** -->
允许的操作：**get**，**set**

<!-- The base address of the vDSO mapping, or zero. -->
vDSO映射的基地址值，或为0。

### ZX_PROP_JOB_IMPORTANCE

<!-- *handle* type: **Job** -->
*handle*类型：**Job**

<!-- *value* type: **zx_job_importance_t** -->
*value*类型：**size_t**

<!-- Allowed operations: **get**, **set** -->
允许的操作：**get**，**set**

<!-- A hint about how important a job is; used to rank jobs for the out-of-memory
(OOM) killer. -->
关于某个任务的重要性的提示值，用于在内存不足（OOM）而需强制杀死任务时为任务排序。

<!-- Additional errors: -->
其他错误码：

<!-- *   **ZX_ERR_OUT_OF_RANGE**: If the importance value is not valid -->
* **ZX_ERR_OUT_OF_RANGE**：如果该重要性值无效

### ZX_PROP_SOCKET_RX_BUF_MAX

<!-- *handle* type: **Socket** -->
*handle*类型：**Socket**

<!-- *value* type: **size_t** -->
*value*类型：**size_t**

<!-- Allowed operations: **get** -->
允许的操作：**get**

<!-- The maximum size of the receive buffer of a socket, in bytes. The receive
buffer may become full at a capacity less than the maximum due to overheads. -->
Socket接收缓冲区的最大容量大小，以字节为单位。 
由于额外的开销，接收缓冲区的最大可用容量实际可能小于该最大值。

### ZX_PROP_SOCKET_RX_BUF_SIZE

<!-- *handle* type: **Socket** -->
*handle*类型：**Socket**

<!-- *value* type: **size_t** -->
*value*类型：**size_t**

<!-- Allowed operations: **get** -->
允许的操作：**get**

<!-- The size of the receive buffer of a socket, in bytes. -->
Socket接收缓冲区的大小，以字节为单位。

### ZX_PROP_SOCKET_TX_BUF_MAX

<!-- *handle* type: **Socket** -->
*handle*类型：**Socket**

<!-- *value* type: **size_t** -->
*value*类型：**size_t**

<!-- Allowed operations: **get** -->
允许的操作：**get**

<!-- The maximum size of the transmit buffer of a socket, in bytes. The transmit
buffer may become full at a capacity less than the maximum due to overheads. -->
Socket发送缓冲区的最大容量大小，以字节为单位。 
由于额外的开销，传输缓冲区的最大可用容量实际可能小于该最大值。

### ZX_PROP_SOCKET_TX_BUF_SIZE

<!-- *handle* type: **Socket** -->
*handle*类型：**Socket**

<!-- *value* type: **size_t** -->
*value*类型：**size_t**

<!-- Allowed operations: **get** -->
允许的操作：**get**

<!-- The size of the transmit buffer of a socket, in bytes. -->
Socket发送缓冲区的大小，以字节为单位。

### ZX_PROP_CHANNEL_TX_MSG_MAX

<!-- *handle* type: **Channel** -->
*handle*类型：**Channel**

<!-- *value* type: **size_t** -->
*value*类型：**size_t**

<!-- Allowed operations: **get** -->
允许的操作：**get**

<!-- The maximum number of packets a channel endpoint can have pending in
its outgoing direction. -->
该属性表示通道的端点在其传出方向上可以的排队的最大数据包数量。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_object_get_property**() returns **ZX_OK** on success. In the event of
failure, a negative error value is returned. -->
**zx_object_get_property()** 执行成功则返回**ZX_OK**。
如果发生错误，将返回以下错误码之一。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**: *handle* is not a valid handle -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄

<!-- **ZX_ERR_WRONG_TYPE**: *handle* is not an appropriate type for *property* -->
**ZX_ERR_WRONG_TYPE**：*handle*的类型不适合*property*属性

<!-- 
**ZX_ERR_ACCESS_DENIED**: *handle* does not have the necessary rights for the
operation -->
**ZX_ERR_ACCESS_DENIED**：*handle*缺少需要的操作权限

<!-- **ZX_ERR_INVALID_ARGS**: *value* is an invalid pointer -->
**ZX_ERR_INVALID_ARGS**：*value*是无效指针
<!-- 
**ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。
而用户空间无法处理这个（不太可能发生的）错误。
在将来的构建版本中，将不再出现此错误。

<!-- **ZX_ERR_BUFFER_TOO_SMALL**: *value_size* is too small for *property* -->
**ZX_ERR_BUFFER_TOO_SMALL**：对于*property*属性来说，*value_size*的值太小

<!-- **ZX_ERR_NOT_SUPPORTED**: *property* does not exist -->
**ZX_ERR_NOT_SUPPORTED**：*property*属性不存在

<!-- ## SEE ALSO -->
## 另见

[object_set_property](object_set_property.md)

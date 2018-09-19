# zx_vmo_op_range
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmo_op_range.md)

---
<!-- ## NAME -->
## 名称

<!-- vmo_op_range - perform an operation on a range of a VMO -->
vmo_op_range —— 在VMO上指定的范围内执行操作

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmo_op_range(zx_handle_t handle, uint32_t op,
                            uint64_t offset, uint64_t size,
                            void* buffer, size_t buffer_size);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmo_op_range()** performs cache and memory operations against pages held by the VMO. -->
**vmo_op_range()** 的功能是对VMO持有的页面执行缓存和内存操作。

<!-- *offset* byte offset specifying the starting location for *op* in the VMO's held memory. -->
*offset*是偏移量，指定*op*操作从VMO持有的内存中开始执行的位置。

<!-- *size* length, in bytes, to perform the operation on. -->
*size*是用于执行操作的范围长度，以字节为单位。

<!-- *op* the operation to perform: -->
*op*是请求执行的操作。

<!-- *buffer* and *buffer_size* are currently unused. -->
*buffer*和*buffer_size*目前尚未使用。

<!-- **ZX_VMO_OP_COMMIT** - Commit *size* bytes worth of pages starting at byte *offset* for the VMO.
More information can be found in the [vm object documentation](../objects/vm_object.md).
Requires the *ZX_RIGHT_WRITE* right. -->
**ZX_VMO_OP_COMMIT** —— 为VMO提交以*offset*开始的*size*字节所在的页面。 
在[VM对象文档](../objects/vm_object.md)中可找到更多信息。
该操作需要*ZX_RIGHT_WRITE*权限。

<!-- **ZX_VMO_OP_DECOMMIT** - Release a range of pages previously commited to the VMO from *offset* to *offset*+*size*.
Requires the *ZX_RIGHT_WRITE* right. -->
**ZX_VMO_OP_DECOMMIT** —— 释放先前提交给VMO的从*offset*到*offset* + *size*范围内的页面。
该操作需要*ZX_RIGHT_WRITE*权限。

<!-- **ZX_VMO_OP_LOCK** - Presently unsupported. -->
**ZX_VMO_OP_LOCK** —— 目前尚不支持。

<!-- **ZX_VMO_OP_UNLOCK** - Presently unsupported. -->
**ZX_VMO_OP_UNLOCK** —— 目前尚不支持。

<!-- **ZX_VMO_OP_CACHE_SYNC** - Performs a cache sync operation.
Requires the *ZX_RIGHT_READ* right. -->
**ZX_VMO_OP_CACHE_SYNC** —— 执行缓存同步操作。
该操作需要*ZX_RIGHT_READ*权限。

<!-- **ZX_VMO_OP_CACHE_INVALIDATE** - Performs a cache invalidation operation.
Requires the *ZX_RIGHT_WRITE* right. -->
**ZX_VMO_OP_CACHE_INVALIDATE** —— 执行缓存失效操作。
该操作需要*ZX_RIGHT_READ*权限。

<!-- **ZX_VMO_OP_CACHE_CLEAN** - Performs a cache clean operation.
Requires the *ZX_RIGHT_READ* right. -->
**ZX_VMO_OP_CACHE_CLEAN** —— 执行缓存清理操作。
该操作需要*ZX_RIGHT_READ*权限。

<!-- **ZX_VMO_OP_CACHE_CLEAN_INVALIDATE** - Performs cache clean and invalidate operations together.
Requires the *ZX_RIGHT_READ* right. -->
**ZX_VMO_OP_CACHE_CLEAN_INVALIDATE** —— 同时执行缓存清理和缓存失效操作。 
该操作需要*ZX_RIGHT_READ*权限。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmo_op_range**() returns **ZX_OK** on success. In the event of failure, a negative error
value is returned. -->
**zx_vmo_write()** 调用成功则返回**ZX_OK**，如果发生错误，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_OUT_OF_RANGE**  An invalid memory range specified by *offset* and *size*. -->
**ZX_ERR_OUT_OF_RANGE**：由*offset*和*size*指定了无效的内存范围。

<!-- **ZX_ERR_NO_MEMORY**  Allocations to commit pages for *ZX_VMO_OP_COMMIT* failed. -->
**ZX_ERR_NO_MEMORY**：为*ZX_VMO_OP_COMMIT*页面提交操作的内存分配失败。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a VMO handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是VMO类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have sufficient rights to perform the operation. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有足够的权限来执行操作。

<!-- **ZX_ERR_INVALID_ARGS**  *out* is an invalid pointer, *op* is not a valid
operation, or *size* is zero and *op* is a cache operation. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*out*是无效指针；*op*是无效操作；或*size*为零且*op*是高速缓存操作。

<!-- 
**ZX_ERR_NOT_SUPPORTED**  *op* was *ZX_VMO_OP_LOCK* or *ZX_VMO_OP_UNLOCK*, or
*op* was *ZX_VMO_OP_DECOMMIT* and the underlying VMO does not allow decommiting. -->
**ZX_ERR_NOT_SUPPORTED**：*op*为*ZX_VMO_OP_LOCK*或*ZX_VMO_OP_UNLOCK*；或*op*为*ZX_VMO_OP_DECOMMIT*但底层的VMO不允许解除页面提交。
<!-- ## SEE ALSO -->
## 另见

[vmo_create](vmo_create.md),
[vmo_clone](vmo_clone.md),
[vmo_read](vmo_read.md),
[vmo_write](vmo_write.md),
[vmo_get_size](vmo_get_size.md),
[vmo_set_size](vmo_set_size.md),
[vmo_op_range](vmo_op_range.md).
# zx_bti_pin
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/fc9cc3958ceaacca9447023b8b272f6b648f75ee/docs/syscalls/bti_pin.md)

---
<!-- ## NAME -->
## 名称

<!-- bti_pin - pin pages and grant devices access to them -->
bti_pin —— 固定内存页面，并授权设备访问它们

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_bti_pin(zx_handle_t bti, uint32_t options,
                       zx_handle_t vmo, uint64_t offset, uint64_t size,
                       zx_paddr_t* addrs, size_t addrs_count,
                       zx_handle_t* pmt);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **bti_pin**() pins pages of a VMO (i.e. prevents them from being decommitted
with **[vmo_op_range](vmo_op_range.md)**()) and grants the hardware
transaction ID represented by the BTI the ability to access these pages,
with the permissions specified in *options*. -->
**bti_pin()** 固定VMO页面（即阻止它们通过 **[vmo_op_range()](vmo_op_range.md)** 解除映射），并授权BTI表示的硬件事务ID访问这些页面的能力，以及具有*options*中指定的权限。

<!-- *offset* must be aligned to page boundaries. -->
*offset*必须与页面的边界对齐。

<!-- *options* is a bitfield that may contain one or more of *ZX_BTI_PERM_READ*,
*ZX_BTI_PERM_WRITE*, *ZX_BTI_PERM_EXECUTE*, *ZX_BTI_COMPRESS*, and
*ZX_BTI_CONTIGUOUS*.  In order for the call to succeed, *vmo* must have the
READ/WRITE/EXECUTE rights corresponding to the permissions flags set in
*options*. *ZX_BTI_CONTIGUOUS* is only allowed if *vmo* was allocated via
**zx_vmo_create_contiguous**() or **zx_vmo_create_physical**().
*ZX_BTI_COMPRESS* and *ZX_BTI_CONTIGUOUS* are mutually exclusive. -->
*options*是可包含*ZX_BTI_PERM_READ*，*ZX_BTI_PERM_WRITE*，*ZX_BTI_PERM_EXECUTE*，*ZX_BTI_COMPRESS*和*ZX_BTI_CONTIGUOUS*中的一个或多个的位域。
为了使调用成功，*vmo*必须具有与*options*中设置的权限标志相对应的READ/WRITE/EXECUTE权限。
仅当*vmo*通过**zx_vmo_create_contiguous()** 或**zx_vmo_create_physical()** 分配时，才可使用*ZX_BTI_CONTIGUOUS*。
*ZX_BTI_COMPRESS*和*ZX_BTI_CONTIGUOUS*是互斥的，不可同时出现。

<!-- 
If the range in *vmo* specified by *offset* and *size* contains non-committed
pages, a successful invocation of this function will result in those pages
having been committed.  On failure, it is undefined whether they have been
committed. -->
如果*offset*和*size*指定的*vmo*的范围内包含非提交页面，在成功调用后将同时导致这些页面被提交。
如果调用失败时，它们是否被提交未定义。

<!-- *addrs* will be populated with the device-physical addresses of the requested
VMO pages.  These addresses may be given to devices that issue memory
transactions with the hardware transaction ID associated with the BTI.  The
number of addresses returned depends on whether the *ZX_BTI_COMPRESS* or
*ZX_BTI_CONTIGUOUS* options were given.  It number of addresses will be either -->
*addrs*将填充所请求的VMO页面的设备物理地址，这些地址可以给予使用与BTI相关联的硬件事务ID发出存储器事务的设备。
返回的地址数量取决于是否设置了*ZX_BTI_COMPRESS*或*ZX_BTI_CONTIGUOUS*选项，可能值是下列情况之一：
<!-- 1) If neither is set, one per page (*size*/*PAGE_SIZE*) -->
1) 如果两者都没有设置，则每页对应一个地址（地址数量是*size*/*PAGE_SIZE*个）
<!-- 2) If *ZX_BTI_COMPRESS* is set, *size*/*minimum-contiguity*, rounded up
   (each address representing the a run of *minimum-contiguity* run of bytes,
   with the last one being potentially short if *size* is not a multiple of
   *minimum-contiguity*).  It is guaranteed that all returned addresses will be
   *minimum-contiguity*-aligned.  Note that *minimum-contiguity* is discoverable
   via **[object_get_info](object_get_info.md)**(). -->
2) 如果设置了*ZX_BTI_COMPRESS*标志位，则地址数量是*size*/*minimum-contiguity*个并向上取整（每个地址表示*minimum-contiguity*个字节的内存，如果*size*不是*minimum-contiguity*的倍数则最后一个可能不足）。
调用保证所有返回的地址都是*minimum-contiguity*对齐的。
这里需要注意的是，*minimum-contiguity*可通过 **[object_get_info()](object_get_info.md)** 获得。
<!-- 3) If *ZX_BTI_CONTIGUOUS* is set, the single address of the start of the memory. -->
3) 如果设置了*ZX_BTI_CONTIGUOU*标志位，则是从起始地址处开始的单一地址。

<!-- *addrs_count* is the number of entries in the *addrs* array.  It is an error for
*addrs_count* to not match the value calculated above. -->
*addrs_count*是*addrs*的数组项数量。
*addrs_count*与上面计算的值不匹配将产生错误。

<!-- ## OPTIONS -->
## 选项

<!-- - *ZX_BTI_PERM_READ*, *ZX_BTI_PERM_WRITE*, and *ZX_BTI_PERM_EXECUTE* define the access types
that the hardware bus transaction initiator will be allowed to use.
- *ZX_BTI_COMPRESS* causes the returned address list to contain one entry per
  block of *minimum-contiguity* bytes, rather than one per *PAGE_SIZE*. -->
- *ZX_BTI_PERM_READ*，*ZX_BTI_PERM_WRITE*和*ZX_BTI_PERM_EXECUTE*定义了硬件总线事务发起者允许使用的访问类型。
- *ZX_BTI_COMPRESS*使返回的地址列表在每*minimum-contiguity*字节块中包含一个项，而不是每*PAGE_SIZE*字节一个。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- On success, **bti_pin**() returns *ZX_OK*.  The device-physical addresses of the
requested VMO pages will be written in *addrs*.  A handle to the created Pinned
Memory Token is returned via *pmt*.  When the PMT is no longer needed,
*pmt_unpin*() should be invoked. -->
调用成功时，**bti_pin()** 返回*ZX_OK*。
请求的VMO页面的设备物理地址将写入*addrs*中，并通过*pmt*返回新创建的表示固定内存令牌的句柄。
当不再需要PMT时，应调用*pmt_unpin()* 取消固定。

<!-- In the event of failure, a negative error value is returned. -->
如果调用失败，则返回负的错误码。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *bti* or *vmo* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*bti*或*vmo*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *bti* is not a BTI handle or *vmo* is not a VMO handle. -->
**ZX_ERR_WRONG_TYPE**：*bti*不是BTI类型句柄或*vmo*不是VMO类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED** *bti* or *vmo* does not have the *ZX_RIGHT_MAP*, or
*options* contained a permissions flag corresponding to a right that *vmo* does not have. -->
**ZX_ERR_ACCESS_DENIED**：*bti*或*vmo*没有*ZX_RIGHT_MAP*权限，或者*options*包含的权限在*vmo*中没有对应的标志。

<!-- **ZX_ERR_INVALID_ARGS** *options* is 0 or contains an undefined flag, either *addrs* or *pmt*
is not a valid pointer, *addrs_count* is not the same as the number of entries that would be
returned, or *offset* or *size* is not page-aligned. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一：*options*为0或包含未定义的标志；*addrs*或*pmt*是无效指针；*addrs_count*与返回的项数不同；*offset*或*size*不是页面对齐整型。

<!-- **ZX_ERR_OUT_OF_RANGE** *offset* + *size* is out of the bounds of *vmo*. -->
**ZX_ERR_OUT_OF_RANGE**：*offset*+*size*超出了*vmo*的范围。

<!-- **ZX_ERR_UNAVAILABLE** (Temporary) At least one page in the requested range could
not be pinned at this time. -->
**ZX_ERR_UNAVAILABLE**：无法固定请求范围内的至少一个页面（临时性）。

<!-- **ZX_ERR_NO_MEMORY**  Failure due to lack of memory.
There is no good way for userspace to handle this (unlikely) error.
In a future build this error will no longer occur. -->
**ZX_ERR_NO_MEMORY**：由于内存不足导致的失败。而用户空间无法处理这个（不太可能发生的）错误。在将来的构建版本中，将不再出现此错误。

<!-- ## SEE ALSO -->
## 另见

[bti_create](bti_create.md),
[pmt_unpin](pmt_unpin.md),
[object_get_info](object_get_info.md).

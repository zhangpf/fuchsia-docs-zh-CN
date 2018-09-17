# zx_vmar_protect
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/679b2f9ea950d56a34c40a808dc78a9d45db0917/docs/syscalls/vmar_protect.md)

---
<!-- ## NAME -->
## 名称

<!-- vmar_protect - set protection of virtual memory pages -->
vmar_protect —— 设置虚拟内存页面的保护权限

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_vmar_protect(zx_handle_t handle, uint32_t options,
                            zx_vaddr_t addr, uint64_t len);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **vmar_protect**() alters the access protections for the memory mappings
in the range of *len* bytes starting from *addr*. The *options* argument should
be a bitwise-or of one or more of the following: -->
**vmar_protect()** 的功能是更改从*addr*开始*len*个字节范围内的内存映射的访问保护权限。 
*options*参数是包含以下选项中的一个或多个的位或值：
<!-- - **ZX_VM_PERM_READ**  Map as readable.  It is an error if *vmar*
  does not have *ZX_VM_CAN_MAP_READ* permissions or the *vmar* handle does
  not have the *ZX_RIGHT_READ* right.  It is also an error if the VMO handle
  used to create the mapping did not have the *ZX_RIGHT_READ* right. -->
- **ZX_VM_PERM_READ**：映射区域为可读的。 
  如果*vmar*没有*ZX_VM_CAN_MAP_READ*权限，或*vmar*句柄没有*ZX_RIGHT_READ*权限，则会导致错误。  
  如果用于创建映射的VMO句柄没有*ZX_RIGHT_READ*，则也会导致错误。
<!-- - **ZX_VM_PERM_WRITE**  Map as writable.  It is an error if *vmar*
  does not have *ZX_VM_CAN_MAP_WRITE* permissions or the *vmar* handle does
  not have the *ZX_RIGHT_WRITE* right.  It is also an error if the VMO handle
  used to create the mapping did not have the *ZX_RIGHT_WRITE* right. -->
- **ZX_VM_PERM_WRITE**：映射区域为可写的。 
  如果*vmar*没有*ZX_VM_CAN_MAP_WRITE*权限，或*vmar*句柄没有*ZX_RIGHT_WRITE*权限，则会导致错误。  
  如果用于创建映射的VMO句柄没有*ZX_RIGHT_WRITE*，则也会导致错误。
<!-- - **ZX_VM_PERM_EXECUTE**  Map as executable.  It is an error if *vmar*
  does not have *ZX_VM_CAN_MAP_EXECUTE* permissions or the *vmar* handle does
  not have the *ZX_RIGHT_EXECUTE* right.  It is also an error if the VMO handle
  used to create the mapping did not have the *ZX_RIGHT_EXECUTE* right. -->
- **ZX_VM_PERM_EXECUTE**：映射区域为可执行。
  如果*vmar*没有*ZX_VM_CAN_MAP_EXECUTE*权限，或*vmar*句柄没有*ZX_RIGHT_EXECUTE*权限，则会导致错误。 
  如果用于创建映射的VMO句柄没有*ZX_RIGHT_EXECUTE*权限，则也会导致错误。

<!-- *len* must be page-aligned. -->
*len*必须是页面对齐的。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **vmar_protect**() returns **ZX_OK** on success. -->
**vmar_protect()** 调用成功则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *vmar_handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*vmar_handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *vmar_handle* is not a VMAR handle. -->
**ZX_ERR_WRONG_TYPE**：*vmar_handle*不是VMAR类型的句柄。

<!-- **ZX_ERR_INVALID_ARGS**  *prot_flags* is an unsupported combination of flags
(e.g., **ZX_VM_PERM_WRITE** but not **ZX_VM_PERM_READ**), *addr* is
not page-aligned, *len* is 0, or some subrange of the requested range is
occupied by a subregion. -->
**ZX_ERR_INVALID_ARGS**：下列情况之一，*prot_flags*是一个不受支持的标志组合（例如，设置了**ZX_VM_PERM_WRITE**选项但未设置**ZX_VM_PERM_READ**）；*addr*不是页面对齐；*len*是0；或者是请求的范围的某个子范围被子区域所占用。
<!-- 
**ZX_ERR_NOT_FOUND**  Some subrange of the requested range is not mapped. -->
**ZX_ERR_NOT_FOUND**：请求范围的某些子范围尚未映射。

<!-- **ZX_ERR_ACCESS_DENIED**  *vmar_handle* does not have the proper rights for the
requested change, the original VMO handle used to create the mapping did not
have the rights for the requested change, or the VMAR itself does not allow
the requested change. -->
**ZX_ERR_ACCESS_DENIED**：*vmar_handle*对所请求的更改没有适当的权限，用于创建映射的原始VMO句柄没有所请求更改的权限，或者VMAR本身不允许所请求的更改。

<!-- ## NOTES -->
## 注释

<!-- ## SEE ALSO -->
## 另见

[vmar_allocate](vmar_allocate.md),
[vmar_destroy](vmar_destroy.md),
[vmar_map](vmar_map.md),
[vmar_unmap](vmar_unmap.md).
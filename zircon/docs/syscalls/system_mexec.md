# zx_system_mexec
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9b1d42b6f62ed4a4fe443eb03e020c74abcc8875/docs/syscalls/system_mexec.md)

---
<!-- ## NAME -->
## 名称

<!-- zx_system_mexec - Soft reboot the system with a new kernel and bootimage -->
zx_system_mexec —— 使用新内核和启动映像软启动系统

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>

zx_status_t zx_system_mexec(zx_handle_t resource,
                            zx_handle_t kernel_vmo,
                            zx_handle_t bootimage_vmo);
```

<!-- ## DESCRIPTION -->
## 描述

<!-- **zx_system_mexec**() accepts two vmo handles: *kernel_vmo* should contain a
kernel image and *bootimage_vmo* should contain an initrd whose address shall
be passed to the new kernel as a kernel argument. -->
**zx_system_mexec()** 接受两个vmo句柄，其中：*kernel_vmo*包含内核映像，*bootimage_vmo*包含initrd，其地址应作为内核参数传递给新内核。

<!-- To supplant the running kernel, a *resource* of *ZX_RSRC_KIND_ROOT* must be
supplied. -->
要取代正在运行的内核，必须提供*ZX_RSRC_KIND_ROOT*类型的*resource*。

<!-- Upon success, *zx_system_mexec* shall supplant the currently running kernel
image with the kernel image contained within *kernel_vmo*, load the ramdisk
contained within *bootimage_vmo* to a location in physical memory and branch
directly into the new kernel while providing the address of the loaded initrd
to the new kernel. -->
执行成功后，*zx_system_mexec*将取代当前运行的内核映像。新内核映像包含在*kernel_vmo*中，将*bootimage_vmo*中包含的ramdisk加载到物理内存中的某个位置，并直接跳转到新内核，同时将加载的initrd的地址提供给新内核。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **zx_system_mexec**() shall not return upon success. -->
**zx_system_mexec()** 执行成功将不再返回。
# zx_smc_call
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/f268a97954e2a2a7e5f8d94b0fc1f85600770edf/docs/syscalls/smc_call.md)

---
<!-- ## NAME -->
## 名称

<!-- smc_call - Make Secure Monitor Call (SMC) from user space -->
smc_call —— 从用户空间进行安全监控器调用(SMC)

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/smc.h>

typedef struct zx_smc_parameters {
    uint32_t func_id;
    uint64_t arg1;
    uint64_t arg2;
    uint64_t arg3;
    uint64_t arg4;
    uint64_t arg5;
    uint64_t arg6;
    uint16_t client_id;
    uint16_t secure_os_id;
} zx_smc_parameters_t;

typedef struct zx_smc_result {
    uint64_t arg0;
    uint64_t arg1;
    uint64_t arg2;
    uint64_t arg3;
} zx_smc_result_t;

zx_status_t zx_smc_call(zx_handle_t handle,
                        const zx_smc_parameters_t* parameters,
                        zx_smc_result_t* out_smc_result);
```

<!-- ## DESCRIPTION -->
## 描述
<!-- 
**smc_call**() makes a Secure Monitor Call (SMC) from user space. It supports the ARM SMC Calling
Convention using the *zx_smc_parameters_t* input parameter and *zx_smc_result_t* output parameter.
The input *handle* must be a resource object with sufficient privileges in order to be executed. -->
**smc_call()** 的功能是从用户空间进行安全监视器调用（SMC）。 它使用*zx_smc_parameters_t*类型的输入参数和*zx_smc_result_t*类型的输出参数去支持ARM的SMC调用约定。 
输入参数*handle*必须是具有足够权限的资源对象才能执行该调用。

<!-- The majority of the parameters are opaque from **smc_call** perspective because they are
dependent upon the *func_id*. The *func_id* informs the Secure Monitor the service and function
to be invoked. The *client_id* is an optional field intended for secure software to track and
index the calling client OS. The *secure_os_id* is an optional field intended for use when there
are multiple secure operating systems at S-EL1, so that the caller may specify the intended
secure OS. -->
从**smc_call**的角度看，大多数参数都是不透明的(opaque)，因为它们依赖于*func_id*。 
*func_id*通知安全监视器要调用的服务和函数。 
*client_id*是一个可选字段，用于安全软件跟踪和索引调用的客户OS。
*secure_os_id*也是可选字段，用于在S-EL1上有多个安全操作系统时使用，以便于调用者可以指定预期的安全OS。

<!-- More information is available in the [ARM SMC Calling Convention documentation](
http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.den0028b/index.html). -->
更多信息请参见[ARM的SMC调用约定文档](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.den0028b/index.html)。

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **smc_call**() returns ZX_OK if *handle* has sufficient privilege. The
return value of the smc call is returned via **out_smc_result** on success. In the event of
failure, a negative error value is returned. -->
如果*handle*句柄具有足够的权限，**smc_call()** 成功则返回**ZX_OK**，并通过*out_smc_result*返回smc调用的返回值。如果调用失败，则返回下列错误码之一。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE**  *handle* is not a valid handle. -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_WRONG_TYPE**  *handle* is not a resource handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是资源类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED**  *handle* does not have sufficient privileges. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有足够的权限。

<!-- **ZX_ERR_NOT_SUPPORTED**  smc_call is not supported on this system. -->
**ZX_ERR_NOT_SUPPORTED**：此系统上不支持smc_call。

<!-- **ZX_ERR_INVALID_ARGS**  *parameters* or *out_smc_result* a null pointer -->
**ZX_ERR_INVALID_ARGS**：*parameters*或*out_smc_result*是空指针。
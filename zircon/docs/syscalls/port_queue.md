# zx_port_queue
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/d17af78df889107ed5035b3f420567675a3c6ee5/docs/syscalls/port_queue.md)

---
<!-- ## NAME -->
## 名称

<!-- port_queue - queue a packet to an port -->
port_queue —— 将数据包排队到端口

<!-- ## SYNOPSIS -->
## 概要

```
#include <zircon/syscalls.h>
#include <zircon/syscalls/port.h>

zx_status_t zx_port_queue(zx_handle_t handle, const zx_port_packet_t* packet);

```

<!-- ## DESCRIPTION -->
## 描述

<!-- **port_queue**() queues a *packet* to the port specified
by *handle*. -->
**port_queue()** 将*packet*数据包排队到*handle*指定的端口。

```
typedef struct zx_port_packet {
    uint64_t key;
    uint32_t type;
    int32_t status;
    union {
        zx_packet_user_t user;
        zx_packet_signal_t signal;
    };
} zx_port_packet_t;

```

<!-- In *packet* *type* should be **ZX_PKT_TYPE_USER** and only the **user**
union element is considered valid: -->
*packet*结构体中的*type*字段必须是**ZX_PKT_TYPE_USER**，并且联合字段中只有**user**是有效的：

```
typedef union zx_packet_user {
    uint64_t u64[4];
    uint32_t u32[8];
    uint16_t u16[16];
    uint8_t   c8[32];
} zx_packet_user_t;

```

<!-- ## RIGHTS -->
## 权限

TODO(ZX-2399)

<!-- ## RETURN VALUE -->
## 返回值

<!-- **port_queue**() returns **ZX_OK** on successful queue of a packet. -->
**port_queue()** 数据包成功排队则返回**ZX_OK**。

<!-- ## ERRORS -->
## 错误码

<!-- **ZX_ERR_BAD_HANDLE** *handle* isn't a valid handle -->
**ZX_ERR_BAD_HANDLE**：*handle*是无效句柄。

<!-- **ZX_ERR_INVALID_ARGS** *packet* is an invalid pointer. -->
**ZX_ERR_INVALID_ARGS**：*packet*是无效指针。

<!-- **ZX_ERR_WRONG_TYPE** *handle* is not a port handle. -->
**ZX_ERR_WRONG_TYPE**：*handle*不是端口类型句柄。

<!-- **ZX_ERR_ACCESS_DENIED** *handle* does not have **ZX_RIGHT_WRITE**. -->
**ZX_ERR_ACCESS_DENIED**：*handle*没有**ZX_RIGHT_WRITE**权限。

<!-- **ZX_ERR_SHOULD_WAIT** the port has too many pending packets. Once a thread
has drained some packets a new **port_queue**() call will likely succeed. -->
**ZX_ERR_SHOULD_WAIT**：端口有太多待处理的数据包，只有线程排空一些数据包后，后续的**port_queue()** 调用才可能会成功。

<!-- ## NOTES -->
## 注释

<!-- The queue is drained by calling **port_wait**(). -->
通过调用**port_wait()** 来排空队列。


<!-- ## SEE ALSO -->
## 另见

<!-- [port_create](port_create.md).
[port_wait](port_wait.md). -->

[port_create](port_create.md)，[port_wait](port_wait.md)。
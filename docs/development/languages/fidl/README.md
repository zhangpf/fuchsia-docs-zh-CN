# Fidl
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/8f5805551f4d8db26e8f5911bd91c3c3596714a8/development/languages/fidl/README.md)

---
<!-- Fidl is the IPC system for Fuchsia. -->
Fidl是Fuchsia的进程间通信(IPC)子系统

<!-- ## Compiler -->
## 编译器

<!-- ## Language -->
## 语言

<!-- ### Readability rubric -->
### 可读性编程规范
<!-- Fuchsia has adopted a [readability rubric](../../api/fidl.md) for FIDL libraries. -->

Fuchsia为FIDL库设计了[可读性编程规范（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/development/api/fidl.md)。
## Bindings

### C

<!-- - [Documentation](c.md)
- [Echo server example](https://fuchsia.googlesource.com/garnet/+/master/examples/fidl/echo2_server_c/) -->

- [文档](c.md)
- [Echo server示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo2_server_c)


### C++

<!-- - [Documentation](cpp.md)
- [Echo server example](https://fuchsia.googlesource.com/garnet/+/master/examples/fidl/echo2_server_cpp/)
- [Echo client example](https://fuchsia.googlesource.com/garnet/+/master/examples/fidl/echo2_client_cpp/) -->

- [文档](c.md)
- [Echo server示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo2_server_cpp)
- [Echo client示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo2_client_cpp)

### Dart

<!-- - [Echo server example](https://fuchsia.googlesource.com/topaz/+/master/examples/fidl/echo_server_dart/)
- [Echo client example](https://fuchsia.googlesource.com/topaz/+/master/examples/fidl/echo_client_dart/) -->
- [Echo server示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo_server_dart)
- [Echo client示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo_client_dart)

### Go

<!-- - [Echo server example](https://fuchsia.googlesource.com/garnet/+/master/examples/fidl/echo2_server_go/)
- [Echo client example](https://fuchsia.googlesource.com/garnet/+/master/examples/fidl/echo2_client_go/) -->

- [Echo server示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo2_server_go)
- [Echo client示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo2_client_go)

### Rust

<!-- - [Echo server example](https://fuchsia.googlesource.com/garnet/+/master/examples/fidl/echo2_server_rust/)
- [Echo client example](https://fuchsia.googlesource.com/garnet/+/master/examples/fidl/echo2_client_rust/) -->
- [Echo server示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo2_server_rust)
- [Echo client示例](https://github.com/fuchsia-mirror/garnet/tree/master/examples/fidl/echo2_client_rust)

<!-- ## Learning -->
## 学习
<!-- See the [tutorial](tutorial.md) to learn about Fidl service development. -->
请参阅[tutorial](tutorial.md)以了解Fidl服务的开发。

<!-- ## FIDL Tuning Proposals -->
## FIDL修改提案

（*译者注：FIDL的FTP类似于Python的PEP和Rust的RFC*）
<!-- Substantial changes to FIDL (whether the language, the wire format, or
language bindings) are described in [FIDL Tuning Proposals]. These
decisions are recorded here for posterity. This includes both accepted
and rejected designs. [FTP-001] describes the proposal process itself. -->
[FIDL修改提案][FIDL Tuning Proposals]中描述了对FIDL（无论是语言，有线格式还是语言绑定）的重要修改，这些决定记录在这里供后来者参考，包括已接受和被拒绝的设计提案。
[FTP-001][FTP-001]描述了提案流程本身。

[FIDL Tuning Proposals]: ftp/README.md
[FTP-001]: ftp/ftp-001.md
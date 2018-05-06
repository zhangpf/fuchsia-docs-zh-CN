# launchpad
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/a774512b9d926ee438a77ddc6a5f362b71e0cc4b/the-book/launchpad.md)

---
<!---
[launchpad][launchpad] is a Zircon library that provides the
functionality to create and start new processes (including loading ELF
binaries, passing initial RPC messages needed by runtime init, etc).
It is a low-level library and over time it is expected that few pieces
of code will make direct use of it.
--->
[launchpad][launchpad]是Zircon下提供创建和启动新进程功能（包括加载ELF二进制文件，传递初始化运行时所需的初始RPC消息）的系统库。这是一个低级别的系统库，随着时间的推移，预期只有相当少的代码会直接使用它。

<!---
Launchpad is designed to give complete control over the creation of a
new Process. This includes:
- The executable code that will be loaded into the process.
- The initial Handle table of the process.
- The initial set of [file descriptors](libc.md#fds) in the process.
- The process's view into filesystems.
- The unix environment (as in getenv and setenv) of the process.
--->
Launchpad被设计成为创建新进程提供完全的控制，这包括：
- 将可执行代码加载到进程中
- 提供新进程的初始Handle表
- 提供该进程初始的[文件描述符](libc.md)集合
- 进程的文件系统的视图
- unix环境（如getenv和setenv）


<!---
There is extensive documentation about launchpad in [its primary
header file][launchpad-header].
--->
关于launchpad的大量文档在[其主要的头文件][launchpad-header]中。


[launchpad]: https://github.com/fuchsia-mirror/zircon/tree/master/system/ulib/launchpad "launchpad"
[launchpad-header]: https://github.com/fuchsia-mirror/zircon/blob/master/system/ulib/launchpad/include/launchpad/launchpad.h "launchpad header"
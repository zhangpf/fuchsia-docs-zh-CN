# Zircon

---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/README.md)

---

Zircon是驱动Fuchsia操作系统运行的核心平台，它由一个底层微内核（代码位于kernel/...），以及一系列用于包括系统启动，与硬件交互，加载和运行用户态进程的用户态服务、驱动和运行库（代码位于system/...）组成。Fuchsia正是在Zircon的基础上构建了一个更丰富的操作系统。

Zircon官方git仓库位于：https://fuchsia.googlesource.com/zircon

Zircon在Github的只读仓库位于：https://github.com/fuchsia-mirror/zircon

Zircon内核提供了系统调用来管理进程、线程、虚存系统、进程间通信、等待对象状态改变和锁机制（通过futex实现）等。

当前，有一些用于项目早期孵化的临时系统调用，随着长期支持的调用API/ABI稳定下来，在未来将会被剔除。到时预期会有100个左右的系统调用。

Zircon的系统调用通常是非阻塞方式的（non-blocking），但值得一提的是，`wait_one`，`wait_many`，`port_wait`和线程休眠则是例外。

本页面仅是Zircon的一个非全面的文档索引。

+ [入门](getting_started.md)
+ [提交代码补丁](https://github.com/fuchsia-mirror/zircon/tree/master/docs/contributing.md)

+ [概念总览](docs/concepts.md)
+ [内核（Kernel）对象](https://github.com/fuchsia-mirror/zircon/tree/master/docs/objects.md)
+ [进程（Process）对象](https://github.com/fuchsia-mirror/zircon/tree/master/docs/objects/process.md)
+ [进程（Thread）对象](https://github.com/fuchsia-mirror/zircon/tree/master/docs/objects/thread.md)
+ [句柄（Handles）](https://github.com/fuchsia-mirror/zircon/tree/master/docs/handles.md)
+ [系统调用](docs/syscalls.md)

+ [驱动开发工具](docs/ddk/overview.md)

+ [测试](https://github.com/fuchsia-mirror/zircon/tree/master/docs/testing.md)
+ [Hacking notes](https://github.com/fuchsia-mirror/zircon/tree/master/docs/hacking.md)
+ [内存使用分析工具](https://github.com/fuchsia-mirror/zircon/tree/master/docs/memory.md)
+ [与LK的关系](zx_and_lk.md)
+ [微观benchmark测试](https://github.com/fuchsia-mirror/zircon/tree/master/docs/benchmarks/microbenchmarks.md)

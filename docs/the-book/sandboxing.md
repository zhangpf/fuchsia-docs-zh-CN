<!-- # Sandboxing -->
# 沙箱
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/b186c159716fee4ca7eeb198f82f077fb4db40dd/the-book/sandboxing.md)

---

<!-- This document describes how sandboxing works in Fuchsia. -->
本文档描述了沙箱技术在Fushsia系统中的工作原理。

<!-- ## An empty process has nothing -->
## 一个空的进程不具有任何功能
<!-- 
On Fuchsia, a newly created process has nothing. A newly created process cannot
access any kernel objects, cannot allocate memory, and cannot even execute code.
Of course, such a process isn't very useful, which is why we typically create
processes with some initial resources and capabilities.

Most commonly, a process starts executing some code with an initial stack, some
command line arguments, environment variables, a set of initial handles. One of
the most important initial handles is the `PA_VMAR_ROOT`, which the process can
use to map additional memory into its address space. -->

在Fuchsia中，一个新创建的过程不具有任何功能，它无法访问任何内核对象，无法分配内存，甚至无法执行代码。当然，这样的进程不会很有用，所以我们通常需要使用一些初始资源和功能来创建进程。

其中最常见的做法是，进程使用初始堆栈、一些命令行参数、环境变量和一组初始句柄开始执行某些代码，这其中最重要的初始句柄之一是`PA_VMAR_ROOT`，进程可以使用该句柄将额外的内存映射到其地址空间上。

<!-- ## Namespaces are the gateway to the world -->
## 命名空间是通向外部世界的门户

<!-- Some of the initial handles given to a process are directories that the process
mounts into its _namespace_. These handles let the process discover and
communicate with other processes running on the system, including file systems
and other servers. See [Namespaces](namespaces.md) for more details.

The namespace given to a process strongly influences how much of the system the
process can influence. Therefore, configuring the sandbox in which a process
runs amounts to configuring the process's namespace. -->

传入进程的一些初始句柄是进程挂载到*namespace*的目录，这些句柄允许进程发现并与系统上运行的其他进程通信，包括文件系统和其他服务。有关更多详细的信息，请查看[命名空间](namespaces.md)。

赋予进程的命名空间会强烈作用于进程可以对系统发挥的影响。因此，配置进程运行的沙箱相当于配置进程的命名空间。


<!-- ## Packages and namespaces -->
## 包和命名空间
<!-- 
In our current implementation, a process runs in a sandbox if its binary is
contained in a package. As the package manager evolves, these details are
likely to change.

An component run from a package is given access to two namespaces by default:

 * `/svc`, which is a bundle of services from the environment in which the
   component runs.
 * `/pkg`, which is a read-only view of the package containing the component.

A typical component will interact with a number of services from `/svc` in
order to play some useful role in the system.

To access these resources at runtime, a process can use the `/pkg` namespace.
For example, the `root_presenter` can access `cursor32.png` using the absolute
path `/pkg/data/cursor32.png`. -->

在我们当前的实现中，如果进程的二进制文件包含在程序包中，则进程将在沙箱中运行。随着包管理器的发展，这些细节可能会发生变化。

默认情况下，从包运行的组件可以访问两个名称空间：

  * `/svc`：是来自组件运行环境的一组服务。
  * `/pkg`：是包含该组件程序包的只读视图。

一个典型的组件将与`/svc`中的许多服务进行交互，以便在系统中发挥应有的作用。

要在运行时访问这些资源，进程可以使用`/pkg`命名空间。例如，`root_presenter`可以使用绝对路径`/pkg/data/cursor32.png`来访问`cursor32.png`。

<!-- ## Configuring additional namespaces -->
## 配置额外的命名空间

<!-- If a process requires access to additional resources (e.g., device drivers),
the package can request access to additional names by including the `sandbox`
property in its  [Component Manifest](package_metadata.md#Component-Manifest)
for the package. For example, the following `meta/sandbox` file requests
direct access to the input driver: -->

如果进程需要访问额外的资源（例如，设备驱动程序），则程序包可以通过在它自身的[组件清单文件（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/package_metadata.md#component-manifest)中包含`sandbox`属性来请求访问其他额外的名称。例如，以下`meta/sandbox`文件可以请求直接访问输入驱动程序：

```
{
    "dev": [ "class/input" ]
}
```

<!-- In the current implementation, the [AppMgr](../glossary.md#AppMgr) grants all such
requests, but that is likely to change as the system evolves. -->

在当前的实现中，[AppMgr](/docs/glossary.md#appmgr)用于授权所有此类请求，但随着系统的演化，未来可能会发生变化。

<!-- ## Building a package -->
## 构建程序包

<!-- To build a package, use the `package()` macro in `gn` defined in
[`//build/package.gni`](https://fuchsia.googlesource.com/build/+/master/package.gni).
See the documentation for the `package()` macro for details about including resources.

For examples, see [https://fuchsia.googlesource.com/garnet/+/master/packages/prod/fortune]
and [https://fuchsia.googlesource.com/garnet/+/master/bin/fortune/BUILD.gn]. -->

为了构建一个包，请使用`gn`在[`//build/package.gni`](https://github.com/fuchsia-mirror/build/blob/master/package.gni)中定义的`package()`宏。有关资源的详细信息，请参阅`package()`宏的相关文档。

例如，可以参阅[https://github.com/fuchsia-mirror/garnet/blob/master/packages/prod/fortune](https://github.com/fuchsia-mirror/garnet/blob/master/packages/prod/fortune)和[https://github.com/fuchsia-mirror/garnet/blob/master/bin/fortune/BUILD.gn](https://github.com/fuchsia-mirror/garnet/blob/master/packages/prod/fortune)文件。
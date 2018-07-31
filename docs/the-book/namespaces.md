<!-- # Fuchsia Namespaces -->
# Fuchsia命名空间
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/b186c159716fee4ca7eeb198f82f077fb4db40dd/the-book/namespaces.md)

---

<!-- Namespaces are the backbone of file access and service discovery in Fuchsia. -->
在Fuchsia系统中，命名空间是文件访问和服务发现的基础。

<!-- ## Definition -->
## 定义

<!-- A namespace is a composite hierarchy of files, directories, sockets, services,
devices, and other named objects which are provided to a component by its
environment. -->
<!-- Let's unpack that a little bit. -->


命名空间是关于文件、目录、socket套接字、服务、设备和其他命名对象的复合层次结构，这些对象由对应的环境提供给组件。让我们对此定义依次解释：

<!-- **Objects are named**: The namespace contains _objects_ which can be enumerated
and accessed by name, much like listing a directory or opening a file. -->
**对象被命名**：命名空间包含了能够通过名字被枚举和访问的*对象*，这种方式很像其他操作系统上列出目录和打开文件等操作。

<!-- **Composite hierarchy**: The namespace is a _tree_ of objects which has been
assembled by _combining_ together subtrees of objects from other namespaces
into a composite structure where each part has been assigned a path prefix
by convention. -->
**复合层次结构**：命名空间是一个*对象树*，它通过将来自其它命名空间的对象的子树*组合*在一起组成复合结构，其中每个部分按惯例分配了一个路径前缀。

<!-- 
**Namespace per component**: Every component receives its own namespace
tailored to meet its own needs.  It can also publish objects of its own
to be included in other namespaces. -->

**每个组件的命名空间**：每个组件都会收到自己的命名空间，以满足自己的需求。它还可以发布自己的对象，使其被包含其它命名空间中。

<!-- **Constructed by the environment**: The environment which instantiates a
component is responsible for constructing an appropriate namespace for that
component within that scope. -->

**由环境构造**：负责实例化组件的环境在作用域内为该组件构造适当的命名空间。
<!-- 
Namespaces can also be created and used independently from components although
this document focuses on typical component-bound usage. -->
命名空间也可以独立于组件单独创建和使用，而本文档重点介绍其在组件上的典型用法。

<!-- ## Namespaces in Action -->
## 命名空间实战
<!-- 
You have probably already spent some time exploring a Fuchsia namespace;
they are everywhere.  If you type `ls /` at a command-line shell prompt
you will see a list of some of the objects which are accessible from the
shell's namespace.

Unlike other operating systems, Fuchsia does not have a "root filesystem".
As described earlier, namespaces are defined per-component rather than
globally or per-process. -->

你可能已经花了一些时间探索Fuchsia的某个命名空间，因为他们无处不在。如果在命令行shell提示符下键入`ls /`，你将看到一些可从shell命名空间访问的对象的列表。

与其他操作系统不同，Fuchsia没有”根文件系统”。如前所述，命名空间是按组件定义的，而不是全局或按进程定义的。以下是一些有趣的含义：

<!-- 
This has some interesting implications:

- There is no global "root" namespace.
- There is no concept of "running in a chroot-ed environment" because every
  component [effectively has its own private "root"](dotdot.md).
- Components receive namespaces which are tailored to their specific needs.
- Object paths may not be meaningful across namespace boundaries.
- A process may have access to several distinct namespaces at once.
- The mechanisms used to control access to files can also be used to control
  access to services and other named objects on a per-component basis. -->



- 没有全局的“根（root）”命名空间。
- 没有诸如“在chroot环境中运行”的概念，因为每个组件[实际上都有自己的私有“root”（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/dotdot.md)。
- 组件可接收根据其特定需求定制的命名空间。
- 跨命名空间边界的对象路径可能是没有意义的。
- 进程可以同时访问多个不同的命名空间。
- 用于控制对文件的访问的机制还可用于控制对每个组件的服务和其他命名对象的访问。
  
<!-- ## Objects -->
## 对象

<!-- The items within a namespace are called objects.  They come in various flavors,
including:

- Files: objects which contain binary data
- Directories: objects which contain other objects
- Sockets: objects which establish connections when opened, like named pipes
- Services: objects which provide FIDL services when opened
- Devices: objects which provide access to hardware resources -->
  
命名空间中的项称为对象（Object），根据种类不同，它们有各自的特点，包括：

- 文件：包含二进制数据的对象
- 目录：包含其他对象的对象
- socket套接字：打开时建立连接的对象，如命名管道
- 服务：打开时提供FIDL服务的对象
- 设备：提供对硬件资源的访问的对象
  
<!-- ### Accessing Objects -->
### 访问对象
<!-- 
To access an object within a namespace, you must already have another object
in your possession.  A component typically receives channel handles for
objects in the scope of its namespace during
[Namespace Transfer](#namespace-transfer).

You can also create new objects out of thin air by implementing the
appropriate FIDL interfaces.

Given an object's channel, you can open a channel for one of its sub-objects
by sending it a FIDL message which includes an object relative path expression
which identifies the desired sub-object.  This is much like opening files
in a directory.

Notice that you can only access objects which are reachable from the ones
you already have access to.  There is no ambient authority.

We will now define how object names and paths are constructed. -->

要访问命名空间中的对象，你必须已拥有另一个对象。组件通常通过[命名空间传输](#命名空间传输)的方式接收通道（channel）句柄，来访问该命名空间作用域内的对象。你还可以通过实现适当的FIDL接口，凭空创建新对象。

通过给定对象的通道，你可以通过向其发送FIDL消息来打开它某个子对象的通道，该消息包括标识所需子对象的[*对象相对路径表达式*](#对象相对路径表达式)，因此这很像其他操作系统上打开目录中文件的操作。

请注意，你只能访问可从你有权限访问的对象开始访问其它对象，因此避免了[ambient authority](https://en.wikipedia.org/wiki/Ambient_authority)问题。接下来将定义如何构造对象名称和路径。

<!-- ### Object Names -->
### 对象名称
<!-- 
An object name is a locally unique label by which an object can be located
within a container (such as a directory).  Note that the name is a property
of the container's table of sub-objects rather than a property of the object
itself.

For example, `cat` designates a furry object located within some unspecified
recipient of an `Open()` request. 

Objects are fundamentally nameless but they may be called many names by others.

Object names are represented as binary octet strings (arbitrary sequences
of bytes) subject to the following constraints:-->

对象名称是本地对象唯一标签，通过该标签，对象可以位于容器（例如目录）中。请注意，名称是容器本身的子对象表的属性，而不是对象本身的属性。例如，`cat`可以指定一个位于`Open()`请求的某个未指定接收者中的对象。

对象本身是无名的，但是它可能又被其他对象以多个不同的名称调用。对象的名称表示为二进制八位字符串（即任意字节序列），但受以下约束限制：


<!-- - Minimum length of 1 byte.
- Maximum length of 255 bytes.
- Does not contain NULs (zero-valued bytes).
- Does not contain `/`.
- Does not equal `.` or `..`.
- Always compared using byte-for-byte equality (implies case-sensitive). -->
- 最小长度为1个字节。
- 最大长度为255个字节。
- 不包含NUL（零值字节）。
- 不包含`/`。
- 不能等于`.`或`..`。
- 始终使用逐字节进行比较（意味着区分大小写）。
  
<!-- 
Object names are valid arguments to a container's `Open()` method.
See [FIDL Interfaces](#fidl-interfaces).

It is intended that object names be encoded and interpreted as human-readable
sequences of UTF-8 graphic characters, however this property is not enforced
by the namespace itself.

Consequently clients are responsible for deciding how to present names
which contain invalid, undisplayable, or ambiguous character sequences to
the user. -->

对象名是容器的`Open()`方法的有效参数。请参见[FIDL接口](#FIDL接口)一节。对象名称旨在可以编码并解释为UTF-8的人类可读序列，但此属性并不由命名空间本身强制执行。因此，客户端需要负责决定如何向用户呈现可能包含无效、不可显示的或模糊字符序列的对象名称。

<!-- _TODO(jeffbrown): Document a specific strategy for how to present names._ -->
*TODO(jeffbrown)：记录如何呈现名称的具体策略.*

<!-- ### Object Relative Path Expressions -->
### 对象相对路径表达式
<!-- An object relative path expression is an object name or a `/`-delimited
sequence of object names designating a sequence of nested objects to be
traversed in order to locate an object within a container (such as a
directory).

For example, `house/box/cat` designates a furry object located within its
containing object called `box` located within its containing object called
`house` located within some unspecified recipient of an `Open()` request.

An object relative path expression always traverses deeper into the namespace.
Notably, the namespace does not directly support upwards traversal out of
containers (e.g. via `..`) but this feature may be partially emulated by
clients (see below).

Object relative path expressions have the following additional constraints: -->

对象相对路径表达式是对象名称或对象名称的`/`分割序列，指定要遍历的嵌套对象序列，以便在容器（例如目录）中定位对象。例如，`house/box/cat`指定一个位于名为`house`的容器对象中，其名称为`box`的封装对象，同时该对象位于`Open()`请求的某个未指定的接收者中。

对象相对路径表达式总是向下遍历命名空间。而值得注意的是，命名空间不直接支持向上遍历容器（例如，通过`..`），但客户端可以部分模拟此功能（见下文）。对象相对路径表达式具有以下附加约束：
<!-- 
- Minimum length of 1 byte.
- Maximum length of 4095 bytes.
- Does not begin or end with `/`.
- All segments are valid object names.
- Always compared using byte-for-byte equality (implies case-sensitive). -->

- 最小长度为1个字节。
- 最大长度为4095字节。
- 不以`/`开头或结尾。
- 所有段都是有效的对象名称。
- 始终使用逐字节字节进行比较（意味着区分大小写）。
  
<!-- Object relative path expressions are valid arguments to a container's `Open()`
method.  See [FIDL Interfaces](#fidl-interfaces). -->
对象相对路径表达式是容器的`Open()`方法的有效参数，具体请参见[FIDL接口](FIDL接口)。

<!-- ### Client Interpreted Path Expressions -->
### 客户端解释路径表达式

<!-- A client interpreted path expression is a generalization of object relative
path expressions which includes optional features which may be emulated
by client code to enhance compatibility with programs which expect a rooted
file-like interface.

Technically these features are beyond the scope of the Fuchsia namespace
protocol itself but they are often used so we describe them here. -->
客户端解释的路径表达式是对象相对路径表达式的泛化行为，其包括可由客户端代码模拟的可选特征，以增强与具有根文件的类似接口的程序的兼容性。从技术上讲，这些功能超出了Fuchsia命名空间协议本身的范围，但它们经常被使用，所以我们在这里特意一并描述它们。
<!-- 
- A client may designate one of its namespaces to function as its "root".
  This namespace is denoted `/`.
- A client may construct paths relative to its designated root namespace
  by prepending a single `/`.
- A client may construct paths which traverse upwards from containers using
  `..` path segments by folding segments together (assuming the container's
  path is known) through a process known as client-side "canonicalization".
- These features may be combined together. -->
- 客户端可以指定其命名空间之一作为其“root”，该命名空间用`/`表示。
- 客户端可以通过添加单个`/`来构造相对于其指定的根命名空间的路径。
- 客户端可以通过称为客户端“规范化”的过程：将段折叠在一起（假设容器的路径已知），构造使用`..`路径段并从容器自下向上遍历的路径。
- 以上功能可以组合在一起。
  
<!-- 
For example, `/places/house/box/../sofa/cat` designates a furry object
located at `places/house/sofa/cat` within some client designated "root"
container.

Client interpreted path expressions that contain these optional features
are not valid arguments to a container's `Open()` method; they must be
translated by the client prior to communicating with the namespace.
See [FIDL Interfaces](#fidl-interfaces).

For example, `fdio` implements client-side interpretation of `..` paths
in file manipulation APIs such as `open()`, `stat()`, `unlink()`, etc. -->

例如，在某些客户端指定的“root”容器内，`/places/house/box/../sofa/cat`指定了位于`places/house/sofa/cat`的封装对象。

包含这些可选功能的客户端解释路径表达式不是容器的`Open()`方法的有效参数; 它们必须在与命名空间通信之前由客户端进行翻译。具体请参见[FIDL接口](#FIDL接口)。例如，`fdio`在文件操作API中实现了`..`路径的客户端解释，例如这些操作包括`open()`、`stat()`和`unlink()`等。

<!-- ## Namespace Transfer -->
## 命名空间传输

<!-- When a component is instantiated in an environment (e.g. its process is
started), it receives a table which maps one or more namespace path prefixes
to object handles. -->
<!-- 
The path prefixes in the table encode the intended significance of their
associated objects by convention.  For example, the `pkg` prefix should
be associated with a directory object which contains the component's own
binaries and assets.

More on this in the next section. -->

当在环境中实例化组件（例如，启动进程）时，它接收将一个或多个命名空间路径的前缀到对象句柄的映射表，该表中的路径前缀按惯例编码其关联对象到预定意义上。例如，`pkg`前缀应该与包含组件自己的二进制文件和资源的目录对象相关联。

更多相关内容将在下一节中介绍。

<!-- ## Namespace Conventions -->
## 命名空间约定

<!-- This section describes the conventional layout of namespaces for typical
components running on Fuchsia.

The precise contents and organization of a component's namespace varies
greatly depending on the component's role, type, identity, scope,
relation to other components, and rights. See [sandboxing.md] for information
about how namespaces are used to create sandboxes for components.

_For more information about the namespace your component can expect to
receive from its environment, please consult the documentation related to
the component type you are implementing._ -->

本节描述了在Fuchsia上运行的典型组件的常规命名空间布局。相对于其他部件和权限，组件命名空间的准确内容和组织，根据组件的角色、类型、标识和作用域不同而有很大差异。有关如何使用命名空间为组件创建沙箱的信息，请参见[sandboxing.md（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/sandboxing.md)。

*有关组件可以从其环境接收的命名空间的更多信息，请参见与你正在实现的组件类型相关的文档。*

<!-- ### Typical Objects -->
### 典型对象

<!-- There are some typical objects which a component namespace might contain:

- Read-only executables and assets from the component's package.
- Private local persistent storage.
- Private temporary storage.
- Services offered to the component by the system, the component framework,
  or by the client which started it.
- Device nodes (for drivers and privileged components).
- Configuration information. -->

组件的命名空间可能包含一些典型的对象，例如：

- 组件包中的只读可执行文件和资源。
- 私有的本地持久化存储。
- 私有临时存储。
- 系统、组件的框架或者启动组件的客户程序为组件提供的服务。
- 设备节点（为驱动或者特权组件）。
- 配置信息

<!-- ### Typical Directory Structure -->
### 典型目录结构

<!-- 
- `pkg/`: the contents of the current program's package
  - `bin/`: executable binaries within the package
  - `lib/`: shared libraries within the package
  - `data/`: data, such as assets, within the package
- `data/`: local persistent storage (read-write, private to the package)
- `tmp/`: temporary storage (read-write, private to the package)
- `svc/`: services offered to the component
  - `fuchsia.process.Launcher`: launch processes
  - `fuchsia.logger.Log`: log messages
  - `vendor.topic.Interface`: service defined by a _vendor_
- `dev/`: device tree (relevant portions visible to privileged components as needed)
  - `class/`, ...
- `hub/`: introspect the system, see [hub.md] (privileged components only)
- `config/`: configuration data for the component -->

- `pkg/`：当前程序包的内容
  - `bin/`：程序包的可执行二进制文件
  - `lib/`：程序包中的共享库
  - `data/`：程序包中的数据，例如资源等
- `data/`：临时持久存储（可读写，程序包私有）
- `svc/`：为组件提供的服务
  - `fuchsia.process.Launcher`：启动进程
  - `fuchsia.logger.Log`：记录消息
  - `vendor.topic.Interface`：*供应商*定义的服务
- `dev/`：设备树（根据需要对特权组件可见的相关部分）
  - `class/`，......
- `hub/`：内省系统，请参阅[hub.md （英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/hub.md)（仅限特权组件）
- `config/`：组件的配置数据

<!-- ## Namespace Participants -->
## 命名空间参与者

<!-- Here is some more information about a few abstractions which interact with
and support the Fuchsia namespace protocol. -->
以下是一些支持Fuchsia命名空间协议并与之交互的抽象的更多信息。

<!-- ### Filesystems -->
### 文件系统
<!-- 
Filesystems make files available in namespaces.

A filesystem is simply a component which publishes file-like objects which
are included in someone else's namespace. -->

文件系统使得文件在命名空间中可见。而文件系统只是一个发布类文件对象的组件，而这些对象包含在其他的命名空间中。

<!-- ### Services -->
### 服务

<!-- Services live in namespaces.

A service is a well-known object which provides an implementation of a FIDL
interface which can be discovered using the namespace.

A service name corresponds to a path within the `svc` branch of the namespace
from which a component can access an implementation of the service.

For example, the name of the default Fuchsia logging service is
`fuchsia.logger.Log` and its location in the namespace is
`svc/fuchsia.logger.Log`. -->

服务存在于命名空间中。服务是一种众所周知的对象，它提供可以使用命名空间发现的FIDL接口的实现。服务名称对应于命名空间的`svc`分支内的路径，组件可以从该路径访问服务的实现。

例如，Fuchsia默认的日志服务的名称是`fuchsia.logger.Log`，它在命名空间中的位置是`svc/fuchsia.logger.Log`。

<!-- ### Components -->
### 组件

<!-- Components consume and extend namespaces. -->

<!-- A component is an executable program object which has been instantiated
within some environment and given a namespace. -->

<!-- 
A component participates in the Fuchsia namespace in two ways:

1. It can use objects from the namespace which it received from its environment,
   notably to access its own package contents and incoming services.

2. It can publish objects through its environment in the form of a namespace,
   parts of which its environment may subsequently make available to other
   components upon request.  This is how services are implemented by
   components. -->
组件使用并扩展了命名空间。组件是已在某些环境中实例化并给定命名空间的可执行程序对象。组件以如下两种方式参与Fuchsia命名空间：

1. 它可以使用从其环境接收的命名空间中的对象，特别是访问其自己的包内容和传入的服务。
2. 它可以以命名空间的形式通过其环境发布对象，这些命名空间的部分环境随后可根据请求提供给其他组件。
  
以上就是组件实现服务的方式。
<!-- ### Environments -->
### 环境

<!-- Environments construct namespaces.

An environment is a container of components.  Each environment is responsible
for _constructing_ the namespace which its components will receive.

The environment decides what objects a component may access and how the
component's request for services by name will be bound to specific
implementations. -->
环境构建了命名空间。环境是组件的容器，每个环境负责*构造*其组件将收到的命名空间。同时，环境决定了组件可以访问哪些对象，以及组件按名称对服务的请求将如何绑定到特定的实现上。

<!-- ### Configuration -->
### 配置

<!-- Components may have different kinds of configuration data exposed to them
depending on the features listed in their [Component
Manifest](package_metadata.md#Component-Manifest) which are exposed as files in
the /config namespace entry. These are defined by the feature set of the
component. -->

组件可能具有不同类型的配置数据，这具体取决于其[组件清单（英文原文）](https://github.com/fuchsia-mirror/docs/blob/master/the-book/package_metadata.md#component-manifest)中所列出的功能。它们作为`/config`命名空间条目中的文件的方式进行公开，而这些都是由组件的功能集所定义。

<!-- ## FIDL Interfaces -->
## FIDL接口
<!-- _TODO(jeffbrown): Explain how the namespace interfaces work._ -->

*TODO(jeffbrown): 解释命名空间的接口是如何工作的。*
<!-- Fuchsia Boot Sequence -->
Fuchsia启动顺序
=====================

---
[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/03f968808eb88e38a741804ff25c07eb2d6935ae/the-book/boot_sequence.md)

---

<!-- 
This document describes the boot sequence for Fuchsia from the time the Zircon
layer hands control over to the Garnet layer.  This document is a work in
progress that will need to be extended as we bring up more of the system. -->

本文档描述了Fuchsia从Zircon移交控制到Garnet层开始的启动顺序。本文档的完善尚还在进行中，需要我们在启动更多的系统部分后加以扩展。

<!-- # Layer 1: [appmgr](https://fuchsia.googlesource.com/garnet/+/master/bin/appmgr) -->
# 第一层：[appmgr](https://github.com/fuchsia-mirror/garnet/tree/master/bin/appmgr)

<!-- 
`appmgr`'s job is to host the environment tree and help create
processes in these environments.  Processes created by `appmgr`
have an `zx::channel` back to their environment, which lets them create other
processes in their environment and to create nested environments. -->

`appmgr`的任务是驻存系统的环境空间树，以及为环境空间创建进程。通过`appmgr`创建的进程会将`zx::channel`传递回它们的环境空间中，使得它们在环境空间中可以创建其他进程或创建嵌套的环境空间。
<!-- 
At startup, `appmgr` creates an empty root environment and creates
the initial apps listed in `/system/data/appmgr/initial.config` in
that environment. Typically, these applications create environments nested
directly in the root environment. The default configuration contains one initial
app: `bootstrap`. -->

在启动过程中，`appmgr`创建空的根（root）环境空间，并根据`/system/data/appmgr/initial.config`文件的内容在环境空间中创建初始的程序，这些程序通常会在根环境空间中直接创建嵌套的空间。默认的配置中仅包含单独的初始程序：`bootstrap`。

<!-- 
# Layer 2: [sysmgr](https://fuchsia.googlesource.com/garnet/+/master/bin/sysmgr/) -->
# 第二层：[sysmgr](https://github.com/fuchsia-mirror/garnet/tree/master/bin/sysmgr)

<!-- `sysmgr`'s job is to create the boot environment and create a number of
 initial applications in the boot environment. -->

`sysmgr`的任务是创建启动（boot）环境空间，并在该空间中创建数个初始程序。
<!-- 
The services that `sysmgr` offers in the boot environment are not provided by
bootstrap itself. Instead, when `sysmgr` receives a request for a service for
the first time, `sysmgr` lazily creates the appropriate app to implement that
service and routes the request to that app. The table of which applications
implement which services is contained in the
`/system/data/bootstrap/services.config` file. Subsequent requests for the same
service are routed to the already running app. If the app terminates,
`sysmgr` will start it again the next time it receives a request for a
service implemented by that app. -->
`sysmgr`在启动环境空间所提供的服务并不由bootstrap本身所提供。而是当`sysmgr`收到某个服务的第一次请求后，它才会惰性地创建相应的程序来实现该服务，并传递请求到该程序。关于哪些程序实现了哪些服务的说明表包含在`/system/data/bootstrap/services.config`文件中。那么该服务后续的请求将被发送到该启动的程序上。如果程序终止，`sysmgr`在下一次收到该程序所实现的服务请求后，将再次启动该程序。

<!-- `sysmgr` also runs a number of applications in the boot environment at
startup. The list of applications to run at startup is contained in the
`/system/data/bootstrap/apps.config` file. -->
在`sysmgr`启动过程中，它将在启动环境空间中运行了数个程序，这些程序的列表包含在`/system/data/bootstrap/apps.config`文件中。

<!-- # Layer 3: [device_runner](https://fuchsia.googlesource.com/peridot/+/master/bin/device_runner/) -->
# 第三层：[device_runner](https://github.com/fuchsia-mirror/peridot/tree/master/bin/device_runner)

<!-- `device_runner`'s job is to setup the interactive flow for user login and user
management. -->
`device_runner`的任务是为用户登录和用户管理创建交互式流程。


<!-- It first gets access to the root view of the system, starts up Device Shell and
draws the Device Shell UI in the root view starting the interative flow. It also
manages a user database that is exposed to Device Shell via the User Provider
FIDL API. -->
当首次获得系统根视图权限后，`device_runner`启动Device Shell并在根视图下绘制Device Shell视图，启动交互式流程。`device_runner`还管理着用户数据库，并通过User Provider暴露给Device Shell。

<!-- This API allows the Device Shell to add a new user, delete an existing user,
enumerate all existing users and login as an existing user or in incognito mode. -->
该API还允许Device Shell添加新用户、删除用户、枚举所有现有用户和以现有用户或匿名方式登录。

<!-- Adding a new user is done using an Account Manager service that can talk to an
identity provider to get an id token to access the user's
[Ledger](https://fuchsia.googlesource.com/peridot/+/master/bin/ledger/). -->

创建新用户通过账户管理（Account Manager）服务来完成，并与身份供应者交互获取到用户id标志后访问用户的[Ledger](https://github.com/fuchsia-mirror/peridot/tree/master/bin/ledger)。

<!-- Logging-in as an existing user starts an instance of `user_runner` with that
user's id token and with a namespace that is mapped within and managed by
`device_runner`'s namespace. -->

如果现有用户登录系统，将启动`user_runner`实例，该实例带用户id标志，并创建指向`device_runner`的命名空间（namespace），并由其管理的命名空间。

<!-- Logging-in as a guest user (in incognito mode) starts an instance of
`user_runner` but without an id token and a temporary namespace. -->
如果访客用户登录（匿名模式下），将启动带临时命名空间和不具有用户id标志的`user_runner`实例。
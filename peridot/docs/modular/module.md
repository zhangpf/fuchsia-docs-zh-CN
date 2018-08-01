<!-- Module -->
模块
======

---
[*英文原文快照*](https://github.com/fuchsia-mirror/peridot/blob/4fda54363e0bc82f253778a2291016b4fbae120f/docs/modular/module.md)

---

<!-- A module is an application that implements the Module interface, whose lifecycle
is tightly bound to the story in which it was started and may implement a UI. A
module is started in a story by another module or by the system from which it
receives a fuchsia::modular::Link service . It can also receive / provide other services via
ServiceProviders. A module is terminated if the story in which it is running
becomes inactive, or the module that started it decides to terminate it or it
decides to terminate itself. A module can start other modules, create,
send / receive messages and call FIDL interfaces. -->

模块是Fuchsia系统中实现了`Module`接口的应用程序，其生命周期与启动它的Story紧密相关，并且它还可以实现UI。模块由另一个模块或由能够接收`fuchsia::modular::Link`服务的系统在Story中启动， 它还可以通过`ServiceProviders`接收/提供其他服务。如果负责模块运行的Story变为非活动状态，或启动它的模块决定终止它，或它决定自行终止，则该模块终止运行。模块可以启动其他模块、创建、发送/接收消息以及调用FIDL接口。


<!-- ## See also: -->
## 请查看

[Module](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/module/module.fidl)

[fuchsia::modular::ModuleContext](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/module/module_context.fidl)（以前的Story）

[fuchsia::modular::ModuleController](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/module/module_controller.fidl)

[fuchsia::modular::Link](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/story/link.fidl)

[fuchsia::modular::MessageQueue](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/component/message_queue.fidl)
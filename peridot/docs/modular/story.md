Story
=====

---
[*英文原文快照*](https://github.com/fuchsia-mirror/peridot/blob/4fda54363e0bc82f253778a2291016b4fbae120f/docs/modular/story.md)

---

<!-- A story is a logical container for a root application along with associated
data. An instance of a story can be created, deleted, started and stopped by the
system in response to user actions. Creating a new story instance creates an
entry in the user's ledger which stores the data associated with this story
instance; deleting a story instance deletes the associated data. Starting a
story instance runs the root application; which may start other applications. If
the root application is a module, it can start other modules and access / modify
data associated with the story instance (via links). The root application must
also implement the view associated with this story which might embed views from
other applications / modules. -->

Story是根应用程序及其关联数据的逻辑容器。系统可以响应用户操作创建、删除、启动和停止Story实例。 创建新Story实例会在用户的帐目（ledger）中创建一个新的条目项，用于存储与此Story相关联的数据，而删除Stroy实例则会删除关联的数据。启动Story实例将运行根应用程序，该过程可能会启动其他应用程序。如果根应用程序是模块，它可以启动其他模块并（通过链接）访问/修改与Stroy实例关联的数据。根应用程序必须同时实现与此Story相关联的视图，同时该视图可能会嵌入来自其他应用程序/模块的视图。

<!-- ## See also: -->
## 请查看

[fuchsia::modular::StoryProvider](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/story/story_provider.fidl)

[fuchsia::modular::StoryController](https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/story/story_controller.fidl)
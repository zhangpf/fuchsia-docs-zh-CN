# Ledger
---
[*英文原文快照*](https://github.com/fuchsia-mirror/peridot/blob/1e84fcdf8eea07f08f49ef12fd0a2f6ccafc94e3/docs/ledger/README.md)

---

<!-- [TOC] -->
- [Ledger是什么？](#ledger是什么)
- [文档](#文档)
<!-- ## What is Ledger? -->
## Ledger是什么？

<!-- Ledger is a distributed storage system for Fuchsia.

Each application (or more precisely, each [component]) running on behalf of a
particular user has a separate data store provided and managed by Ledger, and
vended to a client application by Fuchsia framework through its [component
context]. -->

Ledger是Fuchsia的分布式存储系统。

每个代表特定用户运行的应用程序（或更确切地说是[组件][cloud provider]）都有一个由Ledger提供和管理的单独数据存储，并由Fuchsia框架通过其[组件上下文][component context]提供到客户应用程序。

<!-- The data store for the particular component/user combination is private - not
accessible to other apps of the same user, and not accessible to other users of
the same app. -->
对于特定组件/用户的组合，其数据存储是私有的——不能被同一用户的其他应用程序访问，以及不能被同一应用程序的其他用户访问。


<!-- Each data store is transparently **synchronized** across devices of its user
through a [cloud provider]. Any data operations are made **offline-first** with
no coordination with the cloud. If concurrent modifications result in a data
conflict, the conflict is resolved using an app-configurable merge policy. -->

数据存储通过[云服务供商][cloud provider]在其用户的设备之间透明地进行**同步**，任何数据操作均采用**离线优先**而不通过云进行协调的策略。如果并发修改导致数据冲突，则使用应用程序可配置的合并策略消解冲突。

<!-- Each data store is organized into collections exposing a **key-value store** API
called *pages*. Page API supports storing data of arbitrary size, atomic changes
across multiple keys, snapshots and modification observers. -->

每个数据存储都被组织成一个集合，并暴露一个名为*pages*的**键值对存储** API。Page API支持存储任意大小的数据，以及跨多个键、快照和修改观察者的原子更改。

<!-- ## Documentation -->
## 文档

<!-- Documentation for using Ledger: -->
使用Ledger的文档：

 <!-- - [User Guide](user_guide.md) -->
 - [使用向导（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/user_guide.md)
  
<!-- Documentation for integrating with Ledger in client apps: -->
集成Ledger到客户应用的相关文档：
<!-- 
 - [API Guide](api_guide.md)
 - [Data Organization](data_organization.md)
 - [Examples](examples.md) -->

 - [API向导（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/api_guide.md)
 - [数据组织（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/data_organization.md)
 - [示例（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/examples.md)
  
<!-- Documentation for setting up a remote Cloud sync provider: -->
用于设置远程云同步提供商的文档：

 <!-- - [Firebase](firebase.md) -->
 - [Firebase（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/firebase.md)

<!-- Documentation for developing Ledger: -->
Ledger开发文档：

 <!-- - [Developer Workflow](workflow.md)
 - [Field Data](field_data.md)
 - [Style Guide](style_guide.md)
 - [Testing](testing.md) -->
 - [开发者流程（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/workflow.md)
 - [成员数据（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/field_data.md)
 - [风格文档（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/style_guide.md)
 - [测试（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/testing.md)
  
<!-- Design documentation: -->
设计文档：

 <!-- - [Architecture](architecture.md)
 - [Conflict Resolution](conflict_resolution.md)
 - [Data in Storage](data_in_storage.md)
 - [Life of a Put](life_of_a_put.md) -->
 - [体系结构（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/architecture.md)
 - [冲突消解（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/conflict_resolution.md)
 - [存储中数据（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/data_in_storage.md)
 - [一次put操作的生命周期（英文原文）](https://github.com/fuchsia-mirror/peridot/blob/master/docs/ledger/life_of_a_put.md)

[cloud provider]: https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.ledger.cloud/cloud_provider.fidl
[component]: https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/action_log/component.fidl
[component context]: https://github.com/fuchsia-mirror/peridot/blob/master/public/fidl/fuchsia.modular/component/component_context.fidl
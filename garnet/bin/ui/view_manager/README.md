<!-- # Mozart View Manager -->
# Mozart视图管理器
---

[*英文原文快照*](https://github.com/fuchsia-mirror/garnet/blob/eadd73dd32daacb88332bc21d666daae5c9d868f/bin/ui/view_manager/README.md)

---
<!-- This directory contains an implementation of the ViewManager interface.
It provides a composable view management system for used by other
applications. -->
该目录包含`ViewManager`接口的实现。它提供了一个可组合的视图管理系统，供其他应用程序使用。

<!-- It doesn't make sense to run this application stand-alone since it
doesn't have any UI of its own to display.  Instead, use the Mozart
Launcher or some other application to launch and embed the UI of some
other application using the view manager. -->

单独运行此应用程序没有意义，因为它没有任何自己的UI可供显示。相反地，需要使用Mozart Launcher或其他一些应用程序通过视图管理器启动和嵌入其他应用程序的UI。
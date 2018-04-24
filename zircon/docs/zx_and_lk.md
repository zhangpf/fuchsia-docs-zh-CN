# Zircon和LK

---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/zx_and_lk.md)

---

<!---
Zircon was born as a branch of [LK](https://github.com/littlekernel/lk) and even
now many inner constructs are based on LK while the layers above are new. For
example, Zircon has the concept of a process but LK does not. However, a Zircon
process is made of LK-level constructs such as LK's ``thread_t``.
--->
从Zircon内部看来，它是构建于[LK](https://github.com/littlekernel/lk)基础之上的，但是其上层却是全新的实现。例如，Zircon具有进程的概念而LK没有，但是Zircon进程却依然构建于LK的结构，比如线程（thread）和内存（memory）之上。

<!---
LK is a Kernel designed for small systems typically used in embedded
applications. It is a good alternative to commercial offerings like
[FreeRTOS](http://www.freertos.org/) or [ThreadX](http://rtos.com/products/threadx/).
Such systems often have a very limited amount of ram, a fixed set of peripherals
and a bounded set of tasks.
--->
LK是为嵌入式程序等小型系统设计的内核。对于[FreeRTOS](http://www.freertos.org/)和[ThreadX](http://rtos.com/products/threadx/)等商业系统，它是不错的开源替代品。这些小型系统通常具有非常有限的内存大小，固定的外围设备和处理的任务规模。

<!---
On the other hand, Zircon targets modern phones and modern personal computers
with fast processors, non-trivial amounts of ram with arbitrary peripherals
doing open ended computation.
--->
而另一方面，Zircon面向的是当前主流的手机和个人电脑，它们通常具备较高性能的处理器，可观的内存容量和丰富的外设以完成各种开放型应用。

<!---
More specifically, some the visible differences are:
--->
更具体地讲，一些可见的区别在于：

<!---
+ LK can run in 32-bit systems. Zircon is 64-bit only.
+ Zircon has first class user-mode support. LK does not.
+ Zircon has a capability-based security model. In LK all code is trusted.
--->
+ 用户模式在Zircon上是一等公民，而LK不支持用户模式；
+ Zircon具有object和handle系统，而LK对两者都不支持；
+ Zircon有基于capability的安全模型，而在LK中，所有的代码都是完全授信的。

<!---
Over time, even the low level constructs have changed to accomodate the new
requirements and to better fit the rest of the system.
--->
随着项目的进展，为了支持新的需求，即使底层结构也可能会发生改变，以更好地适应系统的其他部分。

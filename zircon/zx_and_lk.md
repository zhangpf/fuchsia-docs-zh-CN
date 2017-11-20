# Zircon和LK

LK是为嵌入式程序等小型系统设计的内核。对于[FreeRTOS](http://www.freertos.org/)和[ThreadX](http://rtos.com/products/threadx/)等商业系统，它是不错的开源替代品。这些小型系统通常具有非常有限的内存大小，固定的外围设备和处理的任务规模。

而另一方面，Zircon面向的是当前主流的手机和个人电脑，它们通常具备较高性能的处理器，可观的内存容量和丰富的外设以完成各种开放型应用。

从Zircon内部看来，它是构建于[LK](https://github.com/littlekernel/lk)基础之上的，但是其上层却是全新的实现。例如，Zircon具有进程的概念而LK没有，但是Zircon进程却依然构建于LK的结构，比如线程（thread）和内存（memory）之上。

更具体地讲，一些可见的区别在于：

+ Zircon对用户模式具有头等的支持，但LK却没有；
+ Zircon具有object和handle系统，而LK对两者都不支持；
+ Zircon有基于capability的安全模型，而在LK中，所有的代码都是完全授信的。

随着项目的进展，为了支持新的需求，即使底层结构也可能会发生改变，以更好地适应系统的其他部分。

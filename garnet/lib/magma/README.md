<!-- Magma: Graphics for Zircon -->
Magma：Zircon的图形系统
===========================
---

[*英文原文快照*](https://github.com/fuchsia-mirror/garnet/blob/829716d22f2a71c5eb1563debe9853195f763b98/lib/magma/README.md)

---
<!-- Magma is a framework for graphics drivers on the Zircon kernel. Magma drivers are logically divided into a 'System Driver' which runs as a userspace Zircon driver service, and an 'Application Driver' which runs in the application's address space (this mirrors the architecture of the 'Kernel Mode Driver' and 'User Mode Driver' in traditional graphics stack for monolithic kernels, but here both components run in userspace). -->
Magma是Zircon内核上图形驱动程序的框架。Magma驱动程序在逻辑上被划分为“系统驱动”（以Zircon中用户空间驱动程序服务运行）和“应用程序驱动”（在应用程序的地址空间中运行）（这其实是“内核模式驱动”和“用户模式驱动”的体系结构在宏内核的传统图形堆栈中的反映，但这里两种组件都在用户空间中运行）。

<!-- Magma itself is the body of software that sits between the Application Driver and the System Driver and facilitates communication between the two over Zircon IPC, and provides core buffer sharing logic which underlies the system compositing mechanism.  -->
Magma本身是位于应用驱动和系统驱动之间的软件，通过Zircon的IPC完成两者之间的通信。Magma提供了系统组合机制的基础——核心缓冲区共享逻辑。

<!-- Both the Application Driver and the System Driver interface with the Magma framework through stable, versioned ABIs in order to allow updating the core graphics system and IHV drivers independently of one another. -->

应用程序驱动和系统驱动都通过稳定的版本化ABI与Magma框架连接，以便允许彼此独立地更新核心图形系统和IHV驱动程序。

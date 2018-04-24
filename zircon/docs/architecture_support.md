<!---
# Architecture Support
--->
# 硬件架构支持
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/architecture_support.md)

---
<!---
Fuchsia supports two ISAs: arm64 and x86-64.
--->
Fuchsia支持两种指令集结构：arm64和x86-64。

<!---
## arm64

Fuchsia supports arm64 (also called AArch64) with no restrictions on
supported microarchitectures.
--->
## arm64

Fuchsia在（微结构上）无限制地支持arm64（也称为AArch64）硬件结构。

<!---
## x86-64

Fuchsia supports x86-64 (also called IA32e or AMD64), but with some restrictions
on supported microarchitectures.
--->
## x86-64

Fuchsia支持x86-64（也称IA32e或AMD64），但在微结构的支持上有一些限制。

<!---
### Intel

For Intel CPUs, only Broadwell and later are actively supported and will have new features added for them.  Additionally, we will accept patches to keep Nehalem and later booting.
--->
### Intel

对于Intel处理器，只有Broadwell以及之后的版本才被积极支持，并且会为其增加新功能。另外，我们也接收补丁以保持Nehalem结构或更新版本的启动。

<!---
### AMD

AMD CPUs are not actively supported (in particular, we have no active testing on them), but we will accept patches to ensure correct booting on them.
--->
### AMD

AMD处理器未得到活跃支持（特别是我们没有对其主动进行测试），但是我们会接受补丁来确保能正确启动。

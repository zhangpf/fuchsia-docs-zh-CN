
Fuchsia文档
=======================================

（*译者注：建议来自中国大陆的朋友使用Fuchsia项目在github的[镜像](https://github.com/fuchsia-mirror)*）

该仓库包含了Fuchsia项目的文档

# 我该如何做？

+ [如何上手Fuchsia？][getting_started]

+ [如何使用构建系统？][build_system]

+ [如何配置持续集成？][ci_guide]

+ 如何在下列设备上启动
  + [Acer Switch Alpha 12？][acer_12]
  + [Intel NUC？][intel_nuc]

+ [如何编写flutter模块？][flutter_module]

+ [如何贡献代码变更？][contributing]

# 独立项目文档

+ [Zircon][zircon]

  Zircon是Fuchsia底层的微内核。除此之外，Zircon还包含核心驱动和针对Fuchsia实现的libc。

# 参考资料

+ [一本介绍Fuchsia的书](book.md)

[zircon]: zircon/README.md "Zircon"
[getting_started]: getting_started.md "Getting started"
[build_system]: build_system.md "Build system"
[acer_12]: https://github.com/fuchsia-mirror/zircon/blob/master/docs/targets/acer12.md "Acer 12"
[intel_nuc]: https://github.com/fuchsia-mirror/zircon/blob/master/docs/targets/nuc.md "Intel NUC"
[flutter_module]: https://github.com/fuchsia-mirror/modular/blob/master/examples/HOWTO_FLUTTER.md "Flutter modules"
[ci_guide]: https://github.com/fuchsia-mirror/infra-config/blob/master/README.md "Continuous integration guide"
[contributing]: CONTRIBUTING.md "Contributing changes"

<!-- FIDL: Fuchsia Interface Description Language -->
FIDL：Fuchsia接口描述语言
============================================
---

[*英文原文快照*](https://github.com/fuchsia-mirror/garnet/blob/a01c3dcf48592790417b2f8bf4576675957399b7/public/lib/fidl/README.md)

---
<!-- 
FIDL (formerly known as mojom) is an IDL and encoding format used to describe
*interfaces* to be used on top of zircon message pipes. They are the standard
way applications talk to each other in Fuchsia. -->

FIDL（之前被称为mojom）是一种IDL和编码格式，用于描述在zircon消息管道上使用的*接口*。 
它们是应用程序在Fuchsia中相互通信的标准方式。

<!-- FIDL includes libraries and tools for generating bindings from `.fidl` for
supported languages. Currently, the supported languages include C, C++, Dart,
Go, and Rust. -->

FIDL包含用于为支持的语言从`.fidl`文件中生成绑定的库和工具。 
目前，支持的语言包括C，C++，Dart，Go和Rust。

<!-- ## IDE support -->
## IDE支持

<!-- The [`language-fidl` plugin][language-fidl] provides syntax highlighting for
FIDL files in [Atom][atom]. -->
[`language-fidl`插件][language-fidl]为[Atom][atom]编辑器中的FIDL文件提供了语法高亮显示。


[language-fidl]: https://atom.io/packages/language-fidl "FIDL Atom plugin"
[atom]: https://atom.io "Atom editor"
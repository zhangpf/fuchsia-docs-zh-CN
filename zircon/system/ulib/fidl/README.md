<!-- # The C and C++ fidl library -->
# FIDL的C和C++语言库
----

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/4ef2fb8cc0e5c36934e1377f8f33a3744cc07818/system/ulib/fidl/README.md)

----
<!-- This library provides the runtime for fidl C bindings. This primarily
means the definitions of the message encoding and decoding
functions. This also includes the definitions of fidl data types such
as vectors and strings. -->
该库为FIDL提供C语言绑定的运行时。
其中主要包括消息编码和解码功能的定义，并还包括FIDL的数据类型的定义，例如向量和字符串等。

<!-- ## Dependencies -->
## 依赖

<!-- This library depends only on the C standard library and the Zircon kernel
public API. In particular, this library does not depend on the C++ standard
library, libfbl.a, or libzx.a. -->
该库仅依赖于C标准库和Zircon内核的公开API。
特别地，该库不依赖于C++标准库，以及libfbl.a和libzx.a。

<!-- Some of the object files in this library require an implementation of the
placement new operators. These implementations are typically provided by the
C++ standard library, but they can also be provided by other libraries
(e.g., libzxcpp). -->
该库中的某些目标文件的生成需要new运算符的实现。 
虽然这些实现通常由C++标准库提供，但它们也可以由其他库（例如，libzxcpp）提供。

<!-- ## Idioms -->
## 习惯
<!-- In order to avoid the C++ standard library, this library uses a few unusual
idioms: -->
为了避免使用C++标准库，该库使用了一些不寻常的习惯：

### std::move
<!-- 
Rather than using std::move to create an rvalue reference for a type T, this
library uses static_cast<T&&>, which is the language-level construct (rather
than the library-level construct) for creating rvalue references. -->

不是使用`std::move`为类型`T`创建右值引用，而是使用`static_cast<T&&>`。
`static_cast<T&&>`是用于创建右值引用的语言级（而非库级）构造。

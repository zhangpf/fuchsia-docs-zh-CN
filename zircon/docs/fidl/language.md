<!-- # FIDL: Language Specification -->
# FIDL：语言规范
----

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/9113c330d4d688c97994c179760ed71899d8c70c/docs/fidl/language.md)

----
<!-- This document is a specification of the Fuchsia Interface Definition Language
(FIDL) syntax. -->
本文档是Fuchsia接口定义语言(FIDL)的语法规范。

<!-- See [FIDL: Overview](index.md) for more information about FIDL's overall
purpose, goals, and requirements, as well as links to related documents. -->
有关FIDL的总体目的、目标和要求，以及相关文档的链接的更多信息，请参阅[FIDL概述](index.md)。

<!-- [TOC] -->
- [句法](#句法)
    - [令牌(Token)](#令牌token)
    - [库](#库)
    - [类型和类型声明](#类型和类型声明)
    - [常量声明](#常量声明)
    - [常量表达式](#常量表达式)
- [语法](#语法)

<!-- ## Syntax -->
## 句法
<!-- 
The Fuchsia Interface Definition Language provides a syntax for declaring named
constants, enums, structs, unions, and interfaces. These declarations are
collected into libraries for distribution. -->
Fuchsia接口定义语言提供了声明命名常量、枚举、结构体、联合体和接口的语法。
这些声明被集中到库中以便分发。
<!-- 
FIDL declarations are stored in plain text UTF-8 files. Each file consists of a
sequence of semicolon delimited declarations. The order of declarations within a
FIDL file or among FIDL files within a library is irrelevant. FIDL does not
require (or support) forward declarations of any kind. -->
FIDL声明存储在纯文本UTF-8文件中，每个文件都由一系列以分号分隔的声明组成。 
库中的FIDL文件顺序或FIDL文件中的声明顺序是无关紧要的。
FIDL不要求（或不支持）任何类型的前向声明。

<!-- ### Tokens -->
### 令牌(Token)

<!-- #### Comments -->
#### 注释

<!-- FIDL supports C++-style comments. These go from `//` to the end of the
line. They may contain UTF-8 content (which is of course ignored). -->
FIDL支持C++风格的注释，注释从`//`开始到行尾结束，并包含UTF-8的内容（它们当然会被FIDL编译器作为注释忽略）。

<!-- ```
// this is a comment
struct Foo { // so is this
    int32 f; // and this
}; // last one!
``` -->
```
// 这里是注释
struct Foo { // 这里也是
    int32 f; // 以及这里
}; // 最后一行注释！
```
<!-- #### Document Comments -->
#### 文档注释

<!-- TODO(TO-504): We will generate online documentation from FIDL
files. Perhaps the compiler can emit document contents together with
the declarations in a machine-readable FIDL IR format that could be
consumed by other tools. -->
TODO(TO-504)：我们将从FIDL文件中生成在线文档。
也许编译器可以发布文档内容以及机器可读的FIDL IR格式声明，这些内容和声明可被其他工具使用。

<!-- #### Reserved Words -->
#### 关键字

<!-- The following keywords are reserved in FIDL. -->
FIDL中保留以下关键字。

```
array, as, bool, const, enum, float32, float64, handle, int8, int16,
int32, int64, interface, library, request, string, struct, uint8, uint16,
uint32, uint64, union, using, vector
```

<!-- To use these words as identifiers, they must be escaped by prepending an "@".
For example "interface" is a reserved word but "@interface" is an identifier
whose name is "interface". -->
如果要将这些单词用作标识符，必须通过添加“@”来转义它们。 
例如，`interface`是关键字，但`@interface`是名称为`interface`的标识符。

<!-- #### Identifiers -->
#### 标识符

<!-- FIDL identifiers must match the regex "@?[a-zA-Z_][0-9a-zA-Z]\*". The "@" prefix,
if present, serves to distinguish identifiers from reserved words in the FIDL
language. The "@" prefix itself is ignored for the purposes of naming the
identifier. This allows reserved words in the FIDL language to nevertheless be
used as identifiers (when escaped with "@"). -->
FIDL标识符必须匹配正则表达式`@?[a-zA-Z_][0-9a-zA-Z]*`。 
`@`前缀（如果存在）用于区分FIDL语言中的标识符和关键字，出于命名标识符的目的，`@`前缀本身被忽略，因此这允许FIDL语言中的关键字仍然用作标识符（当使用`@`转义时）使用。

<!-- Identifiers are case-sensitive. -->
标识符区分大小写。

<!-- ```
// a library named "foo"
library foo;

// a struct named "Foo"
struct Foo { };

// a struct named "struct"
struct @struct { };
``` -->
```
// 名为“foo”的库
library foo;

// 名为“Foo”的结构
struct Foo { };

// 名为“struct”的结构
struct @struct { };
```

<!-- #### Qualified Identifiers -->

#### 限定标识符

<!-- FIDL always looks for unqualified symbols within the scope of the current
library. To reference symbols in other libraries, they must be qualified by
prefixing the identifier with the library name or alias. -->
FIDL始终在当前库的作用域内查找未限定的符号。 
要引用其他库中的符号，必须通过在标识符前添加库名或别名来限定它们。

**objects.fidl:**

<!-- ```
    library objects;
    using textures as tex;

    interface Frob {
        // "Thing" refers to "Thing" in the "objects" library
        // "tex.Color" refers to "Color" in the "textures" library
        Paint(Thing thing, tex.Color color);
    };

    struct Thing {
        string name;
    };
``` -->
```
    library objects;
    using textures as tex;

    interface Frob {
        // “Thing”指的是“objects”库中的“Thing”
        // “tex.Color”指的是“textures”库中的“Color”
        Paint(Thing thing, tex.Color color);
    };

    struct Thing {
        string name;
    };
```

**textures.fidl:**

```
    library textures;

    struct Color {
        uint32 rgba;
    };
```

<!-- #### Literals -->
#### 字面量

<!-- FIDL supports the following literal types using C-like syntax: bools, signed
integers, unsigned integers, floats, strings. -->
FIDL支持使用类C语法的以下字面量类型：布尔型，有符号整型，无符号整型，浮点型，字符串。

```
    const bool BOOL = true;
    const int32 INT = -333;
    const uint32 UINT = 42;
    const uint64 DIAMOND = 0x183c7effff7e3c18;
    const string STRING = "a string";
    const float32 FLOAT = 1.0;
```

<!-- #### Declaration Separator -->
#### 声明分隔符
<!-- FIDL uses the semi-colon **';'** to separate adjacent declarations within the
file, much like C. -->
如同C语言一样，FIDL使用分号`;`来分隔文件中的相邻声明。

<!-- ### Libraries -->
### 库

<!-- Libraries are named containers of FIDL declarations. -->
库被称为FIDL声明的容器。

<!-- Each library has a name consisting of a dot-delimited identifier. Library names
appear in [Qualified Identifiers](#qualified-identifiers). -->
每个库都有一个由点分隔的标识符组成的名称。
关于库名称的说明在[限定标识符](#限定标识符)一节中出现。

<!-- Libraries may declare that they use other libraries with a "using" declaration.
This allows the library to refer to symbols defined in other libraries upon which
they depend. Symbols which are imported this way may be accessed either by
qualifying them with the library name as in _"mozart.geometry.Rect"_ or by
qualifying them with the library alias as in _"geo.Rect"_. -->
库使用“using”来声明它们使用的其他库，这允许库引用它们所依赖的其他库中定义的符号。 
以这种方式导入的符号可以通过使用 _`mozart.geometry.Rect`_ 中的库名称限定它们，也可以通过使用库的别名来访问它们，例如 *`geo.Rect`*。

```
    library mozart.composition;
    using mozart.geometry as geo;
    using mozart.buffers;

    interface Compositor { … };
```
<!-- 
In the source tree, each library consists of a directory with some number of
**.fidl** files. The name of the directory is irrelevant to the FIDL compiler
but by convention it should resemble the library name itself. A directory should
not contain FIDL files for more than one library. -->
在源码树中，每个库都有一个包含 **.fidl**文件的目录。 
目录的名称与FIDL编译器无关，但按照约定俗成，它应该类似于库本身的名字。 
一个目录不应包含多个库的FIDL文件。

<!-- The scope of "library" and "using" declarations is limited to a single file.
Each individual file within a FIDL library must restate the "library"
declaration together with any "using" declarations needed by that file. -->
`library`和`using`声明的作用域仅限于单个文件。
FIDL一个库中的每个单独文件必须重新声明`library`及该文件所需的任何`using`声明。

<!-- The library's name may be used by certain language bindings to provide scoping
for symbols emitted by the code generator. -->
某些语言的绑定可以使用库的名称来为代码生成器生成的符号提供相应的作用域范围。

<!-- For example, the C++ bindings generator places declarations for the
FIDL library "fuchsia.ui" within the C++ namespace
"fuchsia::ui". Similarly, for languages such as Dart and Rust which
have their own module system, each FIDL library is compiled as a
module for that language. -->
例如，C++绑定的生成器将`fuchsia.ui`FIDL库的声明放置于`fuchsia::ui`命名空间中。
同样，对于具有自己的模块系统的Dart和Rust等语言，每个FIDL库都被编译为该语言的一个模块。

<!-- ### Types and Type Declarations -->
### 类型和类型声明

<!-- #### Primitives -->
#### 基本类型

<!-- *   Simple value types.
*   Not nullable. -->
*   简单的值类型。
*   不可为空。

<!-- The following primitive types are supported: -->
FIDL支持以下基本类型：

<!-- *    Boolean         **`bool`**
*    Signed integer          **`int8 int16 int32 int64`**
*    Unsigned integer        **`uint8 uint16 uint32 uint64`**
*    IEEE 754 Floating-point **`float32 float64`** -->
*   布尔类型 **`bool`**
*   有符号整型 **`int8 int16 int32 int64`**
*   无符号整型 **`uint8 uint16 uint32 uint64`**
*   IEEE 754浮点类型 **`float32 float64`**
  
<!-- Numbers are suffixed with their size in bits, **`bool`** is 1
byte. -->
数字以它们所占的位数为后缀，**`bool`** 类型为1个字节。

<!-- ##### Use -->
##### 使用

<!-- ```
// A record which contains fields of a few primitive types.
struct Sprite {
    float32 x;
    float32 y;
    uint32 index;
    uint32 color;
    bool visible;
};
``` -->
```
// 包含一些基本类型字段的结构体。
struct Sprite {
    float32 x;
    float32 y;
    uint32 index;
    uint32 color;
    bool visible;
};
```

<!-- #### Enums -->
#### 枚举(Enum)

<!-- *   Proper enumerated types; bit fields are not valid enums.
*   Discrete subset of named values chosen from an underlying integer primitive
    type.
*   Not nullable.
*   Enums must have at least one member. -->

* 枚举是适当的可枚举类型；但位字段是无效的枚举。
* 只可以从基本整型中，选择类型作为命名值的离散子集。
* 不可为`null`。
* 枚举必须至少有一个元素。

<!-- ##### Declaration -->
##### 声明

<!-- The ordinal index is **required** for each enum element. The underlying type of
an enum must be one of: **int8, uint8, int16, uint16, int32, uint32, int64,
uint64**. If omitted, the underlying type is assumed to be **uint32**. -->
每个枚举元素的序号索引是**必须的**。 
枚举的基本类型必须是以下之一：**int8，uint8，int16，uint16，int32，uint32，int64，uint64**。 
如果省略，则假定基本类型为**uint32**。

<!-- ```
// An enum declared at library scope.
enum Beverage : uint8 {
    WATER = 0;
    COFFEE = 1;
    TEA = 2;
    WHISKEY = 3;
};

// An enum declared at library scope.
// Underlying type is assumed to be uint32.
enum Vessel {
    CUP = 0;
    BOWL = 1;
    TUREEN = 2;
    JUG = 3;
};
``` -->
```
// 在库作用域中声明枚举。
enum Beverage : uint8 {
    WATER = 0;
    COFFEE = 1;
    TEA = 2;
    WHISKEY = 3;
};

// 在库作用域中声明枚举。
// 底层基本类型为uint32。
enum Vessel {
    CUP = 0;
    BOWL = 1;
    TUREEN = 2;
    JUG = 3;
};
```

<!-- ##### Use -->
##### 使用

<!-- Enum types are denoted by their identifier, which may be qualified if needed. -->
枚举类型由其标识符表示，如果需要，可以对其进行限定。
<!-- 
```
// A record which contains two enum fields.
struct Order {
    Beverage beverage;
    Vessel vessel;
};
``` -->

```
// 包含两个枚举字段的结构体。
struct Order {
    Beverage beverage;
    Vessel vessel;
};
```

<!-- #### Arrays -->
#### 数组

<!-- *   Fixed-length sequences of homogeneous elements.
*   Elements can be of any type including: primitives, enums, arrays, strings,
    vectors, handles, structs, unions.
*   Not nullable themselves; may contain nullable types. -->
*   同一类型元素的定长序列。
*   元素可以是任何类型，包括：基本类型，枚举，数组，字符串，向量，句柄，结构体，联合体。
*   自己本身不可为`null`，但可以包含null类型。

<!-- ##### Use -->
##### 使用

<!-- Arrays are denoted **`array<T>:n`** where _T_ can
be any FIDL type (including an array) and _n_ is a positive
integer constant expression which specified the number of elements in
the array. -->
数组表示为 **`array<T>:n`**，其中 _T_ 可以是任何FIDL类型（包括数组），_n_ 是正值整数常量表达式，它指定了数组中的元素个数。
<!-- 
```
// A record which contains some arrays.
struct Record {
    // array of exactly 16 floating point numbers
    array<float32>:16 matrix;

    // array of exactly 10 arrays of 4 strings each
    array<array<string>:4>:10 form;
};
``` -->

```
// 包含数组的结构体。
struct Record {
    // 包含16个浮点数的数组
    array<float32>:16 matrix;

    // 包含10个子数组，每个子数组中包含4个字符串
    array<array<string>:4>:10 form;
};
```

<!-- #### Strings -->
#### 字符串

<!-- *   Variable-length sequence of UTF-8 encoded characters representing text.
*   Nullable; null strings and empty strings are distinct.
*   Can specify a maximum size, eg. **`string:40`** for a
    maximum 40 byte string. -->
*   表示文本的UTF-8编码字符的可变长度序列。
*   可以为`null`；`null`字符串和空字符串是不同的。
*   可以指定最大尺寸，例如，**`string:40`**表示包含最多40个字节的字符串。

<!-- ##### Use -->
##### 使用

<!-- Strings are denoted as follows: -->
字符串表示如下：

<!-- *   **`string`** : non-nullable string (validation error
    occurs if null is encountered)
*   **`string?`** : nullable string
*   **`string:N, string:N?`** : string with maximum
    length of _N_ bytes -->

* **`string`**：不可为null的字符串（如果为null，则产生验证错误）
* **`string?`**：可以为null的字符串
* **`string:N, string:N?`**：最大长度为_N_ 个字节的字符串

<!-- ```
// A record which contains some strings.
struct Record {
    // title string, maximum of 40 bytes long
    string:40 title;

    // description string, may be null, no upper bound on size
    string? description;
};
``` -->
```
// 包含一些字符串的结构体。
struct Record {
    // title字符串，最长40个字节
    string:40 title;

    // description字符串，可以为null，且大小没有上限
    string? description;
};
```

<!-- #### Vectors -->
#### 向量

<!-- *   Variable-length sequence of homogeneous elements.
*   Nullable; null vectors and empty vectors are distinct.
*   Can specify a maximum size, eg. **`vector<T>:40`** for a
    maximum 40 element vector.
*   There is no special case for vectors of bools. Each bool element takes one
    byte as usual. -->
*   可变长度的同一类型元素序列。
*   可以为`null`，`null`向量和空向量是不同的。
*   可以指定最大容量，例如。 **`vector<T>:40`**表示最多40个元素的向量。
*   `bool`类型的向量也不例外，每个`bool`元素像往常一样占用一个字节。

<!-- ##### Use -->
##### 使用

<!-- Vectors are denoted as follows: -->
向量的表示如下：

<!-- *   **`vector<T>`** : non-nullable vector of element type
    _T_ (validation error occurs if null is encountered) -->
*   **`vector<T>`**：元素类型 _T_ 的非`null`向量（如果为`null`，则发生验证错误）
<!-- *   **`vector<T>?`** : nullable vector of element type
    _T_ -->
*   **`vector<T>?`**：元素类型 _T_ 的可为`null`向量
<!-- *   **`vector<T>:N, vector<T>:N?`** : vector with
    maximum length of _N_ elements -->
*   **`vector<T>:N, vector<T>:N?`**：最大长度为 _N_ 元素的向量

<!-- ```
// A record which contains some vectors.
struct Record {
    // a vector of up to 10 integers
    vector<int32>:10 params;

    // a vector of bytes, no upper bound on size
    vector<uint8> blob;

    // a nullable vector of up to 24 strings
    vector<string>:24? nullable_vector_of_strings;

    // a vector of nullable strings
    vector<string?> vector_of_nullable_strings;

    // a vector of vectors of arrays of floating point numbers
    vector<vector<array<float32>:16>> complex;
};
``` -->
```
// 包含向量的结构体。
struct Record {
    // 最多包含10个整型的向量
    vector<int32>:10 params;

    // 字节向量，大小没有上限
    vector<uint8> blob;

    // 一个可为null的向量，最多包含24个字符串
    vector<string>:24? nullable_vector_of_strings;

    // 包含可为null字符串的向量
    vector<string?> vector_of_nullable_strings;

    // 包含浮点数组向量的向量
    vector<vector<array<float32>:16>> complex;
};
```

<!-- #### Handles -->
#### 句柄(Handle)

<!-- *   Transfers a Zircon capability by handle value.
*   Stored as a 32-bit unsigned integer.
*   Nullable by encoding as a zero-valued handle. -->
*   通过句柄值传递Zircon的功能。
*   存储为32位无符号整型。
*   通过编码为零值，句柄可表示为空。

<!-- ##### Use -->
##### 使用

<!-- Handles are denoted: -->
句柄可表示为：

<!-- *   **`handle`** : non-nullable Zircon handle of
    unspecified type
*   **`handle?`** : nullable Zircon handle of
    unspecified type
*   **`handle<H>`** : non-nullable Zircon handle
    of type _H_
*   **`handle<H>?`** : nullable Zircon handle of
    type _H_ -->
* **`handle`**：未指定类型的不可为`null`的Zircon句柄
* **`handle?`**：未指定类型的可为`null`的Zircon句柄
* **`handle<H>`**：_H_ 类型不可为`null`的Zircon句柄
* **`handle<H>?`**：_H_ 型可为`null`的Zircon句柄

<!-- _H_ can be one of: `channel, event, eventpair, fifo, job,
process, port, resource, socket, thread, vmo`. New types will
be added to the fidl language as they are added to Zircon. -->
_H_ 可以是以下类型之一：`channel, event, eventpair, fifo, job,
process, port, resource, socket, thread, vmo`。 
如果有新句柄类型被添加到Zircon中，那么它们也会被添加到FIDL语言中。

<!-- ```
// A record which contains some handles.
struct Record {
    // a handle of unspecified type
    handle h;

    // an optional channel
    handle<channel>? c;
};
``` -->
```
// 包含句柄的结构体
struct Record {
    // 未指定类型的句柄
    handle h;

    // 可为null的句柄
    handle<channel>? c;
};
```
<!-- #### Structs -->
#### 结构体

<!-- *   Record type consisting of a sequence of typed fields.
*   Declaration is not intended to be modified once deployed; use interface
    extension instead.
*   Reference may be nullable.
*   Structs contain one or more members. A struct with no members is
    difficult to represent in C and C++ as a zero-sized type. Fidl
    therefore chooses to require all structs to have nonzero size. -->
*   结构体类型由一系列类型字段组成。
*   结构体一旦被部署到系统中，声明应不再被修改；请改用接口进行扩展。
*   引用可以为`null`。
*   结构体包含一个或多个成员。 
    没有元素的结构体很难在C和C++中表示为长度为零的类型。 
    因此，FIDL要求所有结构体都具有非零长度。

<!-- ##### Declaration -->
##### 声明

```
struct Point {
    float32 x;
    float32 y;
};
struct Color {
    float32 r;
    float32 g;
    float32 b;
};
```

<!-- #### Use -->
#### 使用

<!-- Structs are denoted by their declared name (eg. **Circle**) and nullability: -->
结构体由其声明的名称（例如**Circle**）和是否为`null`表示：

<!-- *   **`Circle`** : non-nullable Circle
*   **`Circle?`** : nullable Circle -->
* **`Circle`**：不可为`null`的Circle类型结构体
* **`Circle?`**：可为`null`的Circle类型结构体

<!-- ```
struct Circle {
    bool filled;
    Point center;    // Point will be stored in-line
    float32 radius;
    Color? color;    // Color will be stored out-of-line
    bool dashed;
};
``` -->
```
struct Circle {
    bool filled;
    Point center;    // Point将存储在结构体之内
    float32 radius;
    Color? color;    // Color存储在结构体之外
    bool dashed;
};
```

<!-- #### Unions -->
#### 联合体

<!-- *   Tagged option type consisting of tag field and variadic contents.
*   Declaration is not intended to be modified once deployed; use interface
    extension instead.
*   Reference may be nullable.
*   Unions contain one or more members. A union with no members would have
    no inhabitants and make little sense in a wire format. -->

*   联合体是标签化的可选项类型，包含标签字段和可变参数内容。
*   联合体一旦被部署，声明不应再被修改；如果需要修改，请改用接口进行扩展。
*   引用可以为空。
*   联合体包含一个或多个成员。
    没有成员的联合体没有任何内容，并于有线格式中没有任何意义。

<!-- ##### Declaration -->
##### 声明

```
union Pattern {
    Color color;
    Texture texture;
};
struct Color {
    float32 r;
    float32 g;
    float32 b;
};
struct Texture { string name; };
```

<!-- ##### Use -->
##### 使用
<!-- Union are denoted by their declared name (eg. **Pattern**) and nullability: -->
联合体由其声明的名称（例如**Pattern**）和是否为`null`表示：

<!-- *   **`Pattern`** : non-nullable Shape
*   **`Pattern?`** : nullable Shape -->
* **`Pattern`**：不可为`null`的Pattern
* **`Pattern?`**：可为`null`的Pattern

```
struct Paint {
    Pattern fg;
    Pattern? bg;
};
```

<!-- #### Interfaces -->
#### 接口

<!-- *   Describe methods which can be invoked by sending messages over a channel. -->
*   描述可以通过向通道发送消息来调用的方法。
<!-- *   Methods are identified by their ordinal index. Ordinals must be stated
    explicitly to reduce the chance that developers might break interfaces by
    reordering methods and to help with interface extension and derivation.
    *   Method ordinals are unsigned values in the range **0x00000001** to
        **0x7fffffff**.
    *   The FIDL wire format internally represents ordinals as 32-bit values but
        reserves the range **0x80000000** to **0xffffffff** for protocol control
        messages, so these values cannot be associated with methods. -->
*   方法由它们的序数索引确定。 必须明确说明normalal，以减少开发人员通过重新排序方法破坏接口的机会，并帮助进行接口扩展和派生。
    *   方法序号是**0x00000001**到**0x7fffffff**范围内的无符号数值。
    *   FIDL有线格式在内部将序号表示为32位值，但为协议控制消息保留了从**0x80000000**到  **0xffffffff**区间内数组，因此这些值不能与方法相关联。
<!-- *   Each method declaration states its arguments and results.
    *   If no results are declared, then the method is one-way: no response will
        be generated by the server.
    *   If results are declared (even if empty), then the method is two-way:
        each invocation of the method generates a response from the server.
    *   If only results are declared, the method is referred to as an
        *event*. It then defines an unsolicited message from the server. -->
*   每个方法的声明都表示了其参数和返回值。
    *   如果未声明返回值，则该方法是单向的：服务器不会生成任何响应。
    *   如果声明了返回值（即使为空），则该方法是双向的：每次调用该方法都会从服务端生成响应。
    *   如果仅声明结果，则该方法称为*event*。 
        它而后定义来自服务端的主动发生的消息。
<!-- *   When a client or server of an interface is about to close its side of the
    channel, it may elect to send an **epitaph** message to its peer to indicate
    the disposition of the connection. If sent, the epitaph must be the last
    message delivered through the channel. An epitaph message includes:
    *   32-bit generic status: one of the **ZX_status_t** constants
    *   32-bit protocol-specific code: meaning is left up to the interface in
        question
    *   a string: a human-readable message explaining the disposition -->
*   当接口的客户端或服务端即将关闭其通道的一侧时，它可以选择向其对等端发送**墓志铭(epitaph)** 消息以安排该连接的处置。 如果该消息被发送，则墓志铭消息必须是通过该通道传递的最后一条消息。 墓志铭消息包括：
    * 32位的通用状态：**ZX_status_t**类型的常量之一
    * 32位的协议专用代码：其意义留给有问题的接口
    * 字符串：人类可读的解释其处置方法的消息
<!-- *   **Interface extension:** New methods can be added to existing interfaces as
    long as they do not collide with existing methods. -->
*   **接口扩展**：只要新方法不与现有方法冲突，就可以将新方法添加到现有接口中。
<!-- *   **Interface derivation:** New interfaces can be derived from any number of
    existing interfaces as long as none of their methods use the same ordinals.
    (This is purely a FIDL language feature, does not affect the wire format.) -->
*   **接口派生**：新接口可以从任意数量的现有接口的基础上派生，只要它们的方法都不使用相同的序数。（这纯粹是FIDL语言特性，不影响其有线格式。）

<!-- ##### Declaration -->
##### 声明

```
interface Calculator {
    1: Add(int32 a, int32 b) -> (int32 sum);
    2: Divide(int32 dividend, int32 divisor)
    -> (int32 quotient, int32 remainder);
    3: Clear();
    4: -> OnClear();
};

interface RealCalculator : Calculator {
    1001: AddFloats(float32 a, float32 b) -> (float32 sum);
};

interface Science {
    2001: Hypothesize();
    2002: Investigate();
    2003: Explode();
    2004: Reproduce();
};

interface ScientificCalculator : RealCalculator, Science {
    3001: Sin(float32 x) -> (float32 result);
};
```

<!-- ##### Use -->
##### 使用

<!-- Interfaces are denoted by their name, directionality of the channel, and
optionality: -->
接口由它们的名称，通道的方向性和是否为`null`表示：

<!-- *   **`Interface`** : non-nullable FIDL interface (client
    endpoint of channel)
*   **`Interface?`** : nullable FIDL interface (client
    endpoint of channel)
*   **`request<Interface>`** : non-nullable FIDL interface
    request (server endpoint of channel)
*   **`request<Interface>?`** : nullable FIDL interface request
    (server endpoint of channel) -->
* **`Interface`**：不可为`null`的FIDL接口（通道的客户端点）
* **`Interface?`**：可为`null`的FIDL接口（通道的客户端点）
* **`request<Interface>`**：可为`null`的FIDL接口请求（通道的服务端点）
* **`request<Interface>s?`**：可为`null`的FIDL接口请求（通道的服务端点）

<!-- ```
// A record which contains interface-bound channels.
struct Record {
    // client endpoint of a channel bound to the Calculator interface
    Calculator c;

    // server endpoint of a channel bound to the Science interface
    request<Science> s;

    // optional client endpoint of a channel bound to the
    // RealCalculator interface
    RealCalculator? r;
};
``` -->
```
// 包含接口绑定通道的Record结构体。
struct Record {
    // 绑定到Calculator接口的通道的客户端点
    Calculator c;

    // 绑定到Science接口的通道的服务端点
    request<Science> s;

    // 绑定到RealCalculator接口的通道的可为null的客户端点
    RealCalculator? r;
};
```

<!-- ### Constant Declarations -->
### 常量声明

<!-- Constant declarations introduce a name within their scope. The constant's type
must be either a primitive or an enum. -->
常量声明在其作用域内引入了一个名称。
常量必须是基本或枚举类型。
<!-- ```
// a constant declared at library scope
const int32 FAVORITE_NUMBER = 42;
``` -->

```
// 在库作用域内声明的常量
const int32 FAVORITE_NUMBER = 42;
```

<!-- ### Constant Expressions -->
### 常量表达式
<!-- Constant expressions are either literals or the names of other
constant expressions. -->
常量表达式是字面量或其他常量表达式的名称。

<!-- ## Grammar -->
## 语法

<!-- A modified [EBNF description of the fidl grammar is here](grammar.md). -->

[FIDL语法的经修改的EBNF描述](grammar.md)。
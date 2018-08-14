<!-- # Editor integration for Zircon -->
# Zircon的编辑器集成
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/a9a870064667a36e70930409be31496588cc3f8b/docs/editors.md)

---

## YouCompleteMe
<!-- 
[YouCompleteMe](https://valloric.github.io/YouCompleteMe/) is a semantic
code-completion engine. YouCompleteMe works natively with Vim but it can also be
integrated with other editors through [ycmd](https://github.com/Valloric/ycmd). 
-->

[YouCompleteMe](https://valloric.github.io/YouCompleteMe/)是基于语义的代码补全引擎，它原本是配合vim一起工作的，但它也可以通过[ycmd](https://github.com/Valloric/ycmd)与其他编辑器进行集成。

<!-- 
### Install YouCompleteMe in your editor
-->
### 在你的编辑器中安装YouCompleteMe
<!-- 
See the [installation
guide](https://github.com/Valloric/YouCompleteMe#installation).
-->
见[安装指南](https://github.com/Valloric/YouCompleteMe#installation)。

<!-- **Note**: Installing YCM on MacOS with Homebrew is not recommended because of
library compatibility errors. Use the official installation guide instead. -->
**注意**：由于库的兼容性错误，建议不要使用Homebrew在MacOS上安装YCM，请改用官方的安装指南。

<!-- #### gLinux (Googlers only) -->
#### gLinux（仅Googler有效）
<!-- (This applies to anyone compiling on gLinux, even if editing over SSHFS on
MacOS) Ignore the above. Search the Google intranet for "YouCompleteMe" for
installation instructions. -->
（*译者注：仅对内部googler适用，在翻译中已省略*）

<!-- ### Generate compilation database -->
### 生成编译数据库

<!-- YouCompleteMe (and other tools like clang-tidy) require a [JSON compilation
database](https://clang.llvm.org/docs/JSONCompilationDatabase.html) that
specifies how each file is compiled. This database is normally stored in a file
called `compile_commands.json`. There are multiple ways to generate this
database for Zircon. -->

YouCompleteMe（和其他工具，如clang-tidy）需要[JSON编译数据库](https://clang.llvm.org/docs/JSONCompilationDatabase.html)来指定每个文件的编译方式，该数据库通常存储在名为`compile_commands.json`的文件中。有以下多种方法为Zircon生成该数据库。

#### compiledb-generator
<!-- 
[compiledb-generator](https://github.com/nickdiego/compiledb-generator) is the
recommended way to generate a compilation database. It works by running make in
no-op mode and parsing the output. Thus it may be less accurate than bear, but
it should be able to produce a complete database very quickly and without
requiring a clean build. -->

[compiledb-generator](https://github.com/nickdiego/compiledb-generator)是生成编译数据库的推荐方法，它的工作原理是在no-op模式下运行make并解析输出。所以它可能不如[bear](#bear)准确，但可以非常快速地生成完整的数据库，而不需要一次完整的构建。

<!-- To use it, download it from GitHub and run (ensure that compiledb is in your PATH) -->
要使用它，请从GitHub下载并运行（并确保compiledb在PATH中）

```bash
cd "${ZIRCON_DIR}"
compiledb -o compile_commands.json -n make <make args>
```

#### Bear
<!-- 
Bear intercepts `exec(3)` calls made during a build and scrapes commandlines
that look like C/C++ compiler invocations. Thus it is guaranteed to accurately
capture the commands used during compilation. However it can't incrementally
maintain a compilation database, so it requires performing a clean build. -->
Bear截获在构建期间执行的`exec(3)`调用，并且修改命令行，使其看起来像C/C++编译器的调用，所以它可以确保准确地捕获编译期间使用的命令。但是它无法增量式地维护编译数据库，因此需要执行完整的构建。

<!-- ##### Install Bear -->
##### 安装Bear

<!-- You can try using your system's package manager (apt-get, brew) to install Bear,
but you will need version 2.2.1 or later to match the compiler names that Zircon
uses. Both homebrew's and Debian testing's versions are sufficiently new. You
can also fall back to installing it from
[GitHub](https://github.com/rizsotto/Bear). -->

你可以尝试使用系统的包管理器（如apt-get，brew）来安装Bear，但是需要2.2.1或更高版本的Bear才能匹配到Zircon使用的编译器名称。Homebrew和Debian的testing版本都是足够新的，你也可以选择从[GitHub](https://github.com/rizsotto/Bear)来安装它。

<!-- ##### Invoke Bear on the Zircon build system -->
##### 在Zircon构建系统上调用Bear
<!-- You'll need to do this whenever the sources or makefiles change in a way that
affects includes or types, or when you add/delete/move files, though it doesn't
hurt to use a stale database. -->

每当源代码或makefile发生影响包含头文件或类型的更改时，或者当你添加/删除/移动文件时，你都需要调用Bear，尽管使用过时的数据库并没有什么坏处。

<!-- As easy as: -->
调用Bear非常简单：

```bash
cd "${ZIRCON_DIR}"
make clean
bear make -j$(getconf _NPROCESSORS_ONLN)
```

<!-- This will create a `compile_commands.json` file in the local directory. -->
上述命令将在本地目录中创建`compile_commands.json`文件。

<!-- ### Use it -->
### 使用

<!-- YouCompleteMe will use `compile_commands.json` to do code completion and find
symbol definitions/declarations. See your editor's YouCompleteMe docs for
details. -->
YouCompleteMe使用`compile_commands.json`来补全代码并查找符号的定义/声明。有关这方面使用的详细信息，请参阅编辑器的YouCompleteMe文档。

<!-- 
It should pick up the `json` file automatically. If you want to move it out of
the `zircon` tree, you can move the file to its parent directory. -->
YouCompleteMe可以自动提取`json`文件，如果你想将其移出`zircon`代码树，可以将该文件移动到其父目录。

<!-- ## See also -->
## 另见

<!-- For Fuchsia integration, see
https://fuchsia.googlesource.com/scripts/+/master/vim/README.md -->

有关Fuchsia的Vim集成，请[参阅（英文原文）](https://github.com/fuchsia-mirror/scripts/blob/master/vim/README.md)。
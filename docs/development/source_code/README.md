<!---
**Fuchsia Source
--->

# Fuchsia源码
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/d6693145a6db79ef6db798683c743e640f7d4f96/development/source_code/README.md)

---
<!---
Fuchsia uses the `jiri` tool to manage git repositories
[https://fuchsia.googlesource.com/jiri](https://fuchsia.googlesource.com/jiri).
This tool manages a set of repositories specified by a manifest.
--->
Fuchsia使用`jiri`工具来管理git仓库（
[https://github.com/fuchsia-mirror/jiri](https://github.com/fuchsia-mirror/jiri)），该工具可以通过清单描述文件（manifest）来管理一组git仓库。

<!---
For how to build, see Fuchsia's [Getting Started](/getting_started.md) doc.
--->
关于如何构建源码，请查看[入门](/docs/getting_started.md)文档。

<!---
## Creating a new checkout
--->
## 创建工作目录（和下载源码）

<!---
The bootstrap procedure requires that you have Go 1.6 or newer and Git
installed and on your PATH.
--->
该初始化过程需要系统已经安装了Go（版本为1.6或者更新）和git，并且已经加入到PATH环境变量中。

<!---
First, select the [layer](layers.md) of the system you wish to build. (If you're
unsure, select `topaz`, which contains the lower layers). Then, run the
following command:
--->
首先，选择你想要构建的[layer](layers.md)（如果不知道如何选择，那么请选择`topaz`，它也同时包含了它下面的所有layer(*译者注：包括zircon，peridot和garnet*），然后运行如下命令：

<!---
```
curl -s "https://fuchsia.googlesource.com/scripts/+/master/bootstrap?format=TEXT" | base64 --decode | bash -s <layer>
```
--->
```
curl -s "https://fuchsia.googlesource.com/scripts/+/master/bootstrap?format=TEXT" | base64 --decode | bash -s <layer>
```

<!---
This script will bootstrap a development environment for the given layer
by first creating directories `fuchsia`, then downloading repositories for
`<layer>` as well as dependent repositories required in building `<layer>`.

Upon success, the script should print a message
recommending that you add the `.jiri_root/bin` directory to your PATH. Adding
**jiri** to your PATH is strongly recommended and is assumed by other parts of the
Fuchsia toolchain.
--->
该命令将为该layer初始化一个以`fuchsia`命名的目录作为开发环境，然后下载该`<layer>`包含的所有仓库，以及构建它们所依赖的仓库。

如果脚本运行成功，将会在最后打印一段消息，内容是建议你将`.jiri_root/bin`加入到系统的PATH变量中。强烈建议将**jiri**加入到PATH环境变量中，因为Fuchsia工具链的其他部分已默认这样使用**jiri**。

<!--
### Working without altering your PATH

If you don't like having to mangle your environment variables, and you want
`jiri` to "just work" depending on your current working directory, just copy
`jiri` into your PATH.  However, **you must have write access** (without `sudo`)
to the **directory** into which you copy `jiri`.  If you don't, then `jiri`
will not be able to keep itself up-to-date.
--->
### 不改变PATH变量的工作方式

如果你不想改变你的环境变量，而且`jiri`能根据你的当前工作目录，达到“够用就好”的目的，那么只需将`jiri`拷贝到PATH中的某个目录下。但是请注意，**你必须对该目录具有写权限（**而不是通过sudo的方式），否则，`jiri`将不能更新它自己到最新状态。

<!---
```
cp .jiri_root/bin/jiri ~/bin
```

To use the `fx` tool, you can either symlink it into your `~/bin` directory:

```
ln -s `pwd`/scripts/fx ~/bin
```
--->
```
cp .jiri_root/bin/jiri ~/bin
```

为了使用`fx`工具，你也可以选择创建符号链接到`~/bin`目录：

```
ln -s `pwd`/scripts/fx ~/bin
```

<!---
or just run the tool directly as `scripts/fx`. Make sure you have **jiri** in
your PATH.
--->
或者采用直接运行`scripts/fx`的方式，并确认**jiri**路径已经在PATH系统变量中。

<!---
## Who works on the code
--->
## 谁在维护这些代码

<!---
In the root of every repository and in many other directories are
MAINTAINERS files. These list email addresses of individuals who are
familiar with and can provide code review for the contents of the
containing directory. See [maintainers.md](maintainers.md) for more
discussion.**
--->
在每个仓库的根目录和许多其他目录下，有名为`MAINTAINERS`的文件，这些文件列出了熟悉该代码和为该目录下内容提供代码评阅的开发者邮箱列表。请查看[maintainers.md](https://github.com/fuchsia-mirror/docs/blob/d6693145a6db79ef6db798683c743e640f7d4f96/development/source_code/maintainers.md)以便进一步讨论。

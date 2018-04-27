# Fuchsia
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/d6693145a6db79ef6db798683c743e640f7d4f96/getting_started.md)

---

<!---
Pink + Purple == Fuchsia (a new Operating System)

Welcome to Fuchsia! This document has everything you need to get started with
Fuchsia.
--->
粉红 + 紫色 == 紫红色 （全新的操作系统）

欢迎来到Fuchsia的世界！该文档包含你上手Fuchsia的方方面面。

<!---
*** note
NOTE: The Fuchsia source includes
[Zircon](https://fuchsia.googlesource.com/zircon/+/master/README.md),
the core platform that underpins Fuchsia.
The Fuchsia build process will build Zircon as a side-effect;
to work on Zircon only, read and follow Zircon's
[Getting Started](https://fuchsia.googlesource.com/zircon/+/master/docs/getting_started.md)
doc.
***
--->
>注意：Fuchsia同时包含其底层核心平台[Zircon](zircon/README.md)的代码，并且在构建Fuchsia时，也将一起构建Zircon。如果你仅想专注于Zircon开发，请阅读和关注Zircon的[入门](https://github.com/fuchsia-mirror/zircon/blob/master/docs/getting_started.md)文档。

<!---
## Prerequisites
--->
## 构建前准备

<!---
### Prepare your build environment (Once per build environment)
--->
### 准备你的构建环境（仅需一次）

<!---
### Ubuntu
--->
### Ubuntu

<!---
```
sudo apt-get install texinfo libglib2.0-dev liblz4-tool autoconf libtool libsdl-dev build-essential golang git curl unzip
```
--->
```
sudo apt-get install texinfo libglib2.0-dev liblz4-tool autoconf libtool libsdl-dev golang git build-essential curl unzip
```

<!---
### macOS
--->
### macOS

<!---
1. Install the Xcode Command Line Tools:

```
xcode-select --install
```

1. In addition to the Xcode Command Line tools, you also need to
   install a recent version of the full Xcode.
   Download Xcode from https://developer.apple.com/xcode/.

1. Install the other pre-reqs:
--->
1. 安装Xcode命令行工具（Command Line Tools）：
```
xcode-select --install
```

2. 除了安装Xcode命令行工具之外,还需要安装最新版本的Xcode，下载Xcode的链接：https://developer.apple.com/xcode/。
3. 安装其他的依赖项：

<!---
* Using Homebrew:
```
# Install Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# Install packages
brew install wget pkg-config glib autoconf automake libtool golang
```
--->
* 使用Homebrew:
```
# 安装Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 安装依赖包
brew install wget pkg-config glib autoconf automake libtool golang
```
<!---
* Using MacPorts:

```
# Install MacPorts
# See https://guide.macports.org/chunked/installing.macports.html
port install autoconf automake libtool libpixman pkgconfig glib2
```
--->
* 使用MacPorts:

```
# 安装MacPorts请查看：https://guide.macports.org/chunked/installing.macports.html
port install autoconf automake libtool libpixman pkgconfig glib2
```

<!---
## Build Fuchsia
--->
## 构建Fuchsia

<!---
### Get the source

Follow [the instructions to get the Fuchsia source](/development/source_code/README.md)
and then return to this document.
--->
### 获取代码

在[获取Fuchsia源代码](development/source_code/README.md)后回到本文档。

<!---
### Build

If you added `.jiri_root/bin` to your path as part of getting the source code,
the `fx` command should already be in your path. If not, the command is also
available as `scripts/fx`.
--->
### 构建

如果在获取代码步骤中，如果你已将`.jiri_root/bin`目录加到你的`PATH`中，那么`fx`命令就已在你的路径中。如果不是，也可以通过`scripts/fx`命令来执行。

<!---
```
fx set x64
fx full-build
```
--->
```
fx set x64
fx full-build
```

<!---
The first command selects the build configuration you wish to build and
generates the build system itself in an output directory
(e.g., `out/debug-x64`).
--->
第一条命令选择你想构建的配置类型，然后在输出目录（例如`out/debug-x64`）中生成构建所需的文件。

<!---
The second command actually executes the build, transforming the source code in
build products. If you modify the source tree, you can do an incremental build
by re-running the `fx full-build` command alone.
--->
实际执行构建的是第二条命令，它将源代码转换为构建后的产品。在以后如果你更改了源代码，可通过重新执行`fx full-build`命令进行增量式构建。

<!---
Alternatively, you can use the [underlying build system directly](development/build/README.md).
--->
另外，你也可以直接使用[底层构建系统](https://github.com/fuchsia-mirror/docs/blob/d6693145a6db79ef6db798683c743e640f7d4f96/development/build/README.md)来执行构建。

<!---
#### [optional] Customize Build Environment
--->
#### [可选项] 定制构建环境

<!---
By default you will get a x64 debug build. You can skip this section unless
you want something else.

Run `fset-usage` to see a list of build options. Some examples:
--->
系统默认构建x64体系结构的debug版本，如果你不需要构建其他的配置类型，那么可以跳过该步骤。

运行`fset-usage`可以得到已支持构建类型的列表，例如：

<!---
```
fx set x64                 # x64 debug build
fx set arm64               # arm64 debug build
fx set x64 --release       # x64 release build
```
--->
```
fx set x64                 # 构建x86-64的debug版本
fx set arm64               # 构建arm64的debug版本
fx set x64 --release       # 构建x86-64的release版本
```

<!---
#### [optional] Accelerate builds with `ccache` and `goma`
--->
#### [可选项] 使用`ccache`和`goma`来加速构建

（*译者注：goma仅对内部googler适用，在翻译中已省略*）

<!---
`ccache` accelerates builds by caching artifacts from previous builds. `ccache`
is enabled automatically if the `CCACHE_DIR` environment variable is set and
refers to a directory that exists.
--->
`ccache`通过对上次构建产生的制品进行缓存来加速构建。如果环境变量`CCACHE_DIR`指向已存在的目录，那么将自动开启`ccache`的功能。

<!---
To override the default behaviors, pass flags to `fx set`:
--->
通过向`fx set`传递选项，可以覆盖默认的缓存方式：

<!---
```
--ccache     # force use of ccache even if goma is available
--no-ccache  # disable use of ccache
--no-goma    # disable use of goma
```
--->
```
--ccache     # 强制适用ccache，即使同时也指定了goma
--no-ccache  # 禁用ccache
```

<!---
## Boot Fuchsia
--->
## 启动Fuchsia

<!---
### Installing and booting from hardware
--->
### 在硬件设备上安装和启动

<!---
To get Fuchsia running on hardware requires using the paver, which these
[instructions](/development/workflows/fuchsia_paver.md) will help you get up and running with.
--->
在硬件上运行Fuchsia需要使用paver，使用[该指南](https://github.com/fuchsia-mirror/docs/blob/master/development/workflows/fuchsia_paver.md)会帮助你上手使用它。

<!---
### Boot from QEMU

If you don't have the supported hardware, you can run Fuchsia under emulation
using [QEMU](https://fuchsia.googlesource.com/zircon/+/HEAD/docs/qemu.md).
Fuchsia includes prebuilt binaries for QEMU under `buildtools/qemu`.

The `fx run` command will launch Zircon within QEMU, using the locally built
disk image:
--->
### 从QEMU启动

如果没有已支持的硬件，你也可以通过[QMEU](https://github.com/fuchsia-mirror/zircon/blob/master/docs/qemu.md)仿真的方式运行Fuchsia。Fuchsia在`buildtools/qemu`目录下同时也包含了QEMU的二进制可执行文件。

可以通过`fx run`命令在QEMU中使用本地构建的磁盘镜像来启动Zircon：

<!---
```
fx run
```
--->
```
fx run
```

<!---
There are various flags for `fx run` to control QEMU's configuration:
* `-m` sets QEMU's memory size in MB.
* `-g` enables graphics (see below).
* `-N` enables networking (see below).

Use `fx run -h` to see all available options.
--->
`fx run`有多个可选项来控制QEMU的配置：

* `-m`可设定QEMU使用内存的大小。
* `-g`启用图形界面（详情见后）。
* `-N`启用网络（详情见后）。

使用`fx run -h`可以查看所有支持的选项。

<!---
#### Enabling Graphics
--->
#### 启用图形界面

<!---
Note: Graphics under QEMU are extremely limited due to a lack of Vulkan
support. Only the Zircon UI renders.

To enable graphics under QEMU, add the `-g` flag to `fx run`:
--->
注意： 因为QEMU缺少对Vulkan的支持，QEMU下的图形界面支持是非常有限的，仅仅Zircon UI能被渲染。

为了在QEMU下启动图形界面，你可以在运行`fx run`时增加`-g` 选项：

<!---
```
fx run -g
```
--->
```
fx run -g
```
<!---
#### Enabling Network
--->
#### 启用网络

<!---
First, [configure](https://fuchsia.googlesource.com/zircon/+/master/docs/qemu.md#Enabling-Networking-under-QEMU)
a virtual interface for QEMU's use.

Once this is done you can add the `-N` and `-u` flags to `fx run`:
--->
首先，为QEMU[配置](https://github.com/fuchsia-mirror/zircon/blob/master/docs/qemu.md#enabling-networking-under-qemu)一块虚拟网卡。完成此步骤后，在`fx run`传递`-N`和`-u`选项即可：

<!---
```
fx run -N -u $FUCHSIA_SCRIPTS_DIR/start-dhcp-server.sh
```
--->
```
fx run -N -u scrips/start-dhcp-server.sh
```

<!---
The `-u` flag runs a script that sets up a local DHCP server and NAT to
configure the IPv4 interface and routing.
--->
这里的`-u`选项指定的脚本运行了一个DHCP服务器和NAT网络，并配置了IPv4网络接口和路由。

<!---
## Explore Fuchsia
--->
## 探索Fuchsia

<!---
When Fuchsia has booted and displays the "$" shell prompt, you can run programs!

For example, to receive deep wisdom, run:

```
fortune
```
--->
当Fuchsia启动并且显示"$"命令行提示符时，你即可运行程序了！

例如，你可以运行如下命令~~悟得慧根~~：

```
fortune
```

<!---
### Select a tab
--->
### 选择标签页

<!---
Fuchsia shows multiple tabs after booting. The currently selected tab is
highlighted in yellow at the top of the screen. You can switch to the next
tab using Alt-Tab on the keyboard.
--->
Fuchsia在启动后显示多个标签页（Tab），其中当前选中Tab在屏幕的上方黄色高亮。你可以使用Alt+Tab切换到下一Tab。

<!---
- Tab zero is the console and displays the boot and application log.
- Tabs 1, 2 and 3 contain shells.
- Tabs 4 and higher contain applications you've launched.
--->
- Tab 0是console和显示启动和程序log的界面。
- Tab 1，2和3是shell。
- Tab号大于或等于4是你启动的程序界面

<!---
Note: to select tabs, you may need to enter "console mode". See the next section for details.
--->
注意：你必须进入“console模式”后才能选择Tab，具体方法请参考下一节。

<!---
### Launch a graphical application
--->
### 启动图形应用程序

<!---
QEMU does not support Vulkan and therefore cannot run our graphics stack.

Most graphical applications in Fuchsia use the
[Mozart](https://fuchsia.googlesource.com/garnet/+/master/bin/ui/) system compositor. You can launch
such applications, commonly found in `/system/apps`, like this:
--->
由于QEMU不支持Vulkan，所以尚不能在QEMU上运行图形化程序。

大多数Fuchsia图形应用程序使用[Mozart](https://github.com/fuchsia-mirror/garnet/tree/master/bin/ui)框架，通常可以在`/system/apps`目录下找到， 你可以启动这些程序，例如：

<!---
```
launch spinning_square_view
```
--->
```
launch spinning_square_view
```

<!---
Source code for Mozart example apps is
[here](https://fuchsia.googlesource.com/garnet/+/master/examples/ui).
--->
Mozart的示例程序源代码在该[目录](https://github.com/fuchsia-mirror/garnet/tree/master/examples/ui)。

<!---
When you launch something that uses Mozart, uses hardware-accelerated graphics, or if you build
the [default](https://fuchsia.googlesource.com/topaz/+/master/packages/default) package (which will
boot into the Fuchsia System UI), Fuchsia will enter "graphics mode", which will not display any
of the text shells. In order to use the text shell, you will need to enter "console mode" by
pressing Alt-Escape. In console mode, Alt-Tab will have the behavior described in the previous
section, and pressing Alt-Escape again will take you back to the graphical shell.
--->
如果你想使用Mozart启动程序，请使用图形化硬件加速，或者如果你构建的是[默认](https://github.com/fuchsia-mirror/topaz/blob/master/packages/default)包（将启动到Fuchsia系统界面），Fuchsia将进入“graphics模式”，从而不会显示任何字符shell界面。为了使用shell，你需要按Alt+Escape进入到“console模式”。在console模式下，Alt+Tab和之前提到的具有相同的行为，而再次按Alt+Escape将重新回到图形界面（“graphics模式”）中。

<!---
If you would like to use a text shell inside a terminal emulator from within the graphical shell
you can launch the [term](https://fuchsia.googlesource.com/topaz/+/master/app/term) by selecting the
"Ask Anything" box and typing `moterm`.
--->
如果你想在图形shell中使用字符shell的终端模拟器（Terminal），请选择"Ask Anything"模块，并输入`moterm`来启动[term](https://github.com/fuchsia-mirror/topaz/blob/master/app/term)。


<!---
### Running tests
--->
## 运行测试

<!---
Compiled test binaries are installed in `/system/test/`.
You can run a test by invoking it in the terminal. E.g.
--->
编译后的测试程序安装在`/system/test/`目录下，你可以在终端调用它们来运行测试，例如：

<!---
```
/system/test/ledger_unittests
```
--->
```
/system/test/ledger_unittests
```

<!---
If you want to leave Fuchsia running and recompile and re-run a test, run
Fuchsia with networking enabled in one terminal, then in another terminal, run:
--->
如果你想离开Fuchsia运行环境并重新编译和运行测试，请在一个终端下运行带网络支持的Fuchsia，并在另一个终端运行：

<!---
```
fx run-test <test name> [<test args>]
```
--->
```
fx run-test <test name> [<test args>]
```
<!---
## Contribute changes

* See [CONTRIBUTING.md](CONTRIBUTING.md).
--->
## 贡献代码

* 请查看[贡献代码](CONTRIBUTING.md)文档。

<!---
## Additional helpful documents

* Using Zircon - copying files, network booting, log viewing, and more are [here](https://fuchsia.googlesource.com/zircon/+/master/docs/getting_started.md#Copying-files-to-and-from-Zircon)
* [Fuchsia documentation](/README.md) hub
* More information on the system bootstrap application is
[here](https://fuchsia.googlesource.com/garnet/+/master/bin/sysmgr/).
--->
## 其他一些有用的文档

* 如何[使用Zircon](https://github.com/fuchsia-mirror/zircon/blob/master/docs/getting_started.md#copying-files-to-and-from-zircon)，包括拷贝文件，网络启动，查看日志等。
* [Fuchsia文档主页](README.md)。
* 关于系统引导程序的[更多信息](https://github.com/fuchsia-mirror/garnet/blob/master/bin/sysmgr/)。

# Fuchsia

粉红 + 紫色 == 紫红色 （全新的操作系统）

欢迎来到Fuchsia的世界！该文档包含你上手Fuchsia的方方面面。

>注意：Fuchsia同时包含其底层核心平台[Zircon](zircon/README.md)的代码，并且在构建Fuchsia时，也将一起构建Zircon。如果你仅想专注于Zircon开发，请阅读和关注Zircon的[入门](https://github.com/fuchsia-mirror/zircon/blob/master/docs/getting_started.md)文档。


## 构建前准备

### 准备你的构建环境（仅需一次）

### Ubuntu

```
sudo apt-get install texinfo libglib2.0-dev autoconf libtool libsdl-dev build-essential golang git build-essential curl unzip
```

### macOS

安装Xcode命令行工具（Command Line Tools）：
```
xcode-select --install
```
安装其他的依赖项：

* 使用Homebrew:
```
# 安装Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 安装依赖包
brew install wget pkg-config glib autoconf automake libtool golang
```

* 使用MacPorts:
```
# 安装MacPorts请查看：https://guide.macports.org/chunked/installing.macports.html
port install autoconf automake libtool libpixman pkgconfig glib2
```

## 构建Fuchsia

### 获取代码

在[获取Fuchsia源代码](getting_source.md)后回到本文档。

### 构建

如果在获取代码步骤中，如果你已将`.jiri_root/bin`目录加到你的`PATH`中，那么`fx`命令就已在你的路径中。如果不是，也可以通过`scripts/fx`命令来执行。

```
fx set x86-64
fx full-build
```

第一条命令选择你想构建的配置类型，然后在输出目录（例如`out/debug-x86-64`）中生成构建所需的文件。

实际执行构建的是第二条命令，它将源代码转换为构建后的产品。在以后如果你更改了源代码，可通过重新执行`fx full-build`命令进行增量式构建。

另外，你也可以直接使用[底层构建系统](build_system.md)来执行构建。

#### [可选项] 定制构建环境

系统默认构建x86-64体系结构的debug版本，如果你不需要构建其他的配置类型，那么可以跳过该步骤。

运行`fset-usage`可以得到已支持构建类型的列表，例如：

```
fx set x86-64              # 构建x86-64的debug版本
fx set arm64               # 构建arm64的debug版本
fx set x86-64 --release    # 构建x86-64的release版本
```

#### [可选项] 使用`ccache`和`goma`来加速构建

（*译者注：goma仅对内部googler适用，在翻译中已省略*）

`ccache`通过对上次构建产生的制品进行缓存来加速构建。如果环境变量`CCACHE_DIR`指向已存在的目录，那么将自动开启`ccache`的功能。

通过向`fx set`传递选项，可以覆盖默认的缓存方式：

```
--ccache     # 强制适用ccache，即使同时也指定了goma
--no-ccache  # 禁用ccache
```

## 启动Fuchsia

### 在硬件设备上启动

可以通过以下三种方式在硬件上启动Fuchsia：网络启动（见后）、USB启动（见后）或者在存储设备上[安装](https://github.com/fuchsia-mirror/install-fuchsia/blob/master/README.md)Fuchsia。无论哪种方式，你都需要先拷贝一些启动代码到目标硬件上，为此，使用USB驱动器是一个不错的选择。

如果你想采取网络启动或者USB启动的方式，而不是安装Fuchsia，可使用[build-bootable-usb-gigaboot.sh脚本](https://github.com/fuchsia-mirror/scripts/blob/master/build-bootable-usb-gigaboot.sh)。特别地，如果你采取网络启动方式，请传递 `-m`和`-f`选项以跳过拷贝Zircon内核和Fuchsua系统镜像这两步，因为后面步骤中引导服务器将会完成这两步。

查看下列特定硬件的指引文档将会非常有用，其中Raspberry Pi 3需特别不同的步骤，而其他的指引文档对这些硬件的配置固件会有帮助。

* [Acer Switch Alpha 12](https://github.com/fuchsia-mirror/zircon/blob/master/docs/targets/acer12.md)
* [Intel NUC](https://github.com/fuchsia-mirror/zircon/blob/master/docs/targets/nuc.md)

一旦配置好硬件，你可以通过运行`fx boot`来启动引导服务器。

### 从QEMU启动

如果没有已支持的硬件，你也可以通过[QMEU](https://github.com/fuchsia-mirror/zircon/blob/master/docs/qemu.md)仿真的方式运行Fuchsia。Fuchsia在`buildtools/qemu`目录下同时也包含了QEMU的二进制可执行文件。

可以通过`fx run`命令在QEMU中启动Zircon，同时使用构建好的`user.bootfs`：

```
fx run
```

`fx run`有多个可选项来控制QEMU的配置：

* `-m`可设定QEMU使用内存的大小。
* `-g`启用图形界面（详情见后）。
* `-N`启用网络（详情见后）。

使用`fx run -h`可以查看所有支持的选项。

#### 启用图形界面

为了在QEMU下启动图形界面，你可以在运行`fx run`时增加`-g` 选项：

```
fx run -g
```

#### 启用网络

注意：QEMU模拟器中的网络支持仅对x86_64体系结构有效。

首先，为QEMU[配置](https://github.com/fuchsia-mirror/zircon/blob/master/docs/qemu.md#enabling-networking-under-qemu-x86-64-only)一块虚拟网卡。完成此步骤后，在`fx run`传递`-N`和`-u`选项即可：

```
fx run -N -u scrips/start-dhcp-server.sh
```

这里的`-u`选项指定的脚本运行了一个DHCP服务器和NAT网络，并配置了IPv4网络接口和路由。

## 探索Fuchsia

当Fuchsia启动并且显示"$"命令行提示符时，你即可运行程序了！

例如，你可以运行如下命令~~悟得慧根~~：

```
fortune
```

### 选择标签页

Fuchsia在启动后显示多个标签页（Tab），其中当前选中Tab在屏幕的上方黄色高亮。你可以使用Alt+Tab切换到下一Tab。

- Tab 0是console和显示启动和程序log的界面。
- Tab 1，2和3是shell。
- Tab号大于或等于4是你启动的程序界面

注意：你必须进入“console模式”后才能选择Tab，具体方法请参考下一节。

### 启动图形应用程序

由于QEMU不支持Vulkan，所以尚不能在QEMU上运行图形化程序。大多数Fuchsia图形应用程序使用[Mozart](https://github.com/fuchsia-mirror/garnet/tree/master/bin/ui)框架，通常可以在`/system/apps`目录下找到， 你可以启动这些程序，例如：

```
launch spinning_square_view
```

Mozart的示例程序源代码在该[目录](https://github.com/fuchsia-mirror/garnet/tree/master/examples/ui)。

如果你想使用Mozart启动程序，请使用图形化硬件加速，或者如果你构建的是[默认](https://github.com/fuchsia-mirror/packages/blob/master/gn/default)包（将启动到Fuchsia系统界面），Fuchsia将进入“graphics模式”，从而不会显示任何字符shell界面。为了使用shell，你需要按Alt+Escape进入到“console模式”。在console模式下，Alt+Tab和之前提到的具有相同的行为，而再次按Alt+Escape将重新回到图形界面（“graphics模式”）中。

如果你想在图形shell中使用字符shell的终端模拟器（Terminal），请选择"Ask Anything"模块，并输入`moterm`来启动[moterm](https://github.com/fuchsia-mirror/moterm)。

## 贡献代码

* 请查看[贡献代码](CONTRIBUTING.md)文档。

## 其他一些有用的文档

* 如何[使用Zircon](https://github.com/fuchsia-mirror/zircon/blob/master/docs/getting_started.md#copying-files-to-and-from-zircon)，包括拷贝文件，网络启动，查看日志等。
* [Fuchsia文档主页](README.md)。
* 构建[Fuchsia工具链](toolchain.md)。
* 关于`fx full-build`命令中的[更多细节](build_system.md)。
* 关于系统引导程序的[更多信息](https://github.com/fuchsia-mirror/application/tree/master/src/bootstrap)。

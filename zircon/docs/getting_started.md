# 快速入门指南
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/3adf3875541d28ad944637f753f8e454fa91dceb/docs/getting_started.md)

---

<!---
## Checking out the Zircon source code  
--->
## 检出Zircon代码

<!---
*** note
NOTE: The Fuchsia source includes Zircon. See Fuchsia's
[Getting Started](https://fuchsia.googlesource.com/docs/+/master/getting_started.md)
doc. Follow this doc to work on only Zircon.
***
--->
>注意：由于Fuchsia也包含了Zircon的代码，请查看Fuchsia的[入门指南](../getting_started.md)。如果仅专注于Zircon的开发，请参考此文档。

<!---
The Zircon Git repository is located
at: https://fuchsia.googlesource.com/zircon

To clone the repository, assuming you setup the $SRC variable
in your environment:
```shell
git clone https://fuchsia.googlesource.com/zircon $SRC/zircon
```
--->
Zircon的git仓库位于：[https://fuchsia.googlesource.com/zircon](https://fuchsia.googlesource.com/zircon) (*译者注：Github的镜像[在此](https://github.com/fuchsia-mirror/zircon)*)

假设在环境中已设置好$SRC变量（*译者注：即Fuchsia的工作目录*），克隆Zircon仓库到本地：
```shell
git clone https://fuchsia.googlesource.com/zircon $SRC/zircon
# 或者
git clone https://github.com/fuchsia-mirror/zircon $SRC/zircon
```
<!---
For the purpose of this document, we will assume that Zircon is checked
out in $SRC/zircon and that we will build toolchains, QEMU, etc alongside
that.  Various make invocations are presented with a "-j32" option for
parallel make.  If that's excessive for the machine you're building on,
try -j16 or -j8.
--->
在文档接下来的部分，我们已假设Zircon已经被检出到`$SRC/zircon`目录下，进而工具链、QEMU等也将在`$SRC`下进行构建。多个make命令通过`-j32`选项被并行调用，如果这种并行度对于你的机器比较吃力，请尝试`-j16`或者`-j8`选项。

<!---
## Preparing the build environment
--->
## 准备构建环境

<!---
### Ubuntu

On Ubuntu this should obtain the necessary pre-reqs:
```
sudo apt-get install texinfo libglib2.0-dev autoconf libtool libsdl-dev build-essential
```
--->
### Ubuntu

在Ubuntu系统中，下面的命令可以获取所需的依赖：
```
sudo apt-get install texinfo libglib2.0-dev autoconf libtool libsdl-dev build-essential
```
<!---
### macOS
Install the Xcode Command Line Tools:
```
xcode-select --install
```

Install the other pre-reqs:

* Using Homebrew:
```
brew install wget pkg-config glib autoconf automake libtool
```

* Using MacPorts:
```
port install autoconf automake libtool libpixman pkgconfig glib2
```
--->
### macOS
安装Xcode命令行工具（Command Line Tools）：
```
xcode-select --install
```

安装其他的依赖项：

* 使用Homebrew：
```
brew install wget pkg-config glib autoconf automake libtool
```

* 使用MacPorts：
```
port install autoconf automake libtool libpixman pkgconfig glib2
```
<!---
## Install Toolchains

If you're developing on Linux or macOS, there are prebuilt toolchain binaries avaiable.
Just run this script from your Zircon working directory:

```
./scripts/download-prebuilt
```

If you would like to build the toolchains yourself, follow the instructions later
in the document.
--->
## 安装工具链

如果你的开发环境是Linux或macOS，已有预编译的工具链可直接进行下载，只需在Zircon的工作目录下运行下列脚本即可：

```
./scripts/download-prebuilt
```

如果你想自己构建工具链，请依照本文档后面的步骤执行。

<!---
## Build Zircon

Build results will be in $SRC/zircon/build-{arm64,x64}

The variable $BUILDDIR in examples below refers to the build output directory
for the particular build in question.
--->
## 构建Zircon

构建生成的文件位于`$SRC/zircon/build-{arm64,x64}`下。

对于特定的构建目标，下面示例中的`$BUILDDIR`变量指向构建的输出目录。

<!---
```
cd $SRC/zircon

# for aarch64
make -j32 arm64

# for x64
make -j32 x64
```
--->
```
cd $SRC/zircon

# 对于aarch64
make -j32 arm64

# 对于x64
make -j32 x64
```
<!---
### Using Clang

To build Zircon using Clang as the target toolchain, set the
`USE_CLANG=true` variable when invoking Make.
--->
### 使用Clang

如果你想使用Clang作为构建Zircon的工具链，请在调用make时使用`USE_CLANG=true`选项。
<!---
```
cd $SRC/zircon

# for aarch64
make -j32 USE_CLANG=true arm64

# for x64
make -j32 USE_CLANG=true x64
```
--->
```
cd $SRC/zircon

# 对于aarch64
make -j32 USE_CLANG=true arm64

# 对于x86-64
make -j32 USE_CLANG=true x64
```
<!---
## Building Zircon for all targets

```
# The -r enables release builds as well
./scripts/buildall -r
```

Please build for all targets before submitting to ensure builds work
on all architectures.
--->
## 为所有目标体系结构构建Zircon

```
# -r选项也将同时编译release版本
./scripts/buildall -r
```

请在提交代码变更之前为所有目标体系结构构建一次，已保证代码在所有体系结构上能工作。

<!---
## QEMU

You can skip this if you're only testing on actual hardware, but the emulator
is handy for quick local tests and generally worth having around.

See [QEMU](qemu.md) for information on building and using QEMU with zircon.
--->
## QEMU

如果你使用真实硬件进行测试，那么可以跳过此步骤，但是模拟器可以很方便地进行快速本地测试，所以该步骤通常是值得进行的。

对于在zircon中构建和使用QEMU，请查看[相应的文档（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/qemu.md)。

<!---
## Build Toolchains (Optional)

If the prebuilt toolchain binaries do not work for you, there are a
set of scripts which will download and build suitable gcc toolchains
for building Zircon for the ARM64 and x86-64 architectures:
--->
## 构建Toolchains（可选项）

如果预编译工具链对于你的系统不适用，为了在ARM64和x86-64上构建Zircon，也有一些脚本可用于你下载和构建合适的gcc工具链：
<!---
```
cd $SRC
git clone https://fuchsia.googlesource.com/third_party/gcc_none_toolchains toolchains
cd toolchains.
./do-build --target arm-none
./do-build --target aarch64-none
./do-build --target x86_64-none
```
--->
```
cd $SRC
git clone https://fuchsia.googlesource.com/third_party/gcc_none_toolchains toolchains
cd toolchains.
./do-build --target arm-none
./do-build --target aarch64-none
./do-build --target x86_64-none
```

<!---
### Configure PATH for toolchains

If you're using the prebuilt toolchains, you can skip this step, since
the build will find them automatically.
--->
### 为toolchains配置PATH环境变量

如果使用的是预编译工具链，构建过程可以自动找到它们，因此可跳过此步骤。

<!---
```
# on Linux
export PATH=$PATH:$SRC/toolchains/aarch64-elf-5.3.0-Linux-x86_64/bin
export PATH=$PATH:$SRC/toolchains/x86_64-elf-5.3.0-Linux-x86_64/bin

# on Mac
export PATH=$PATH:$SRC/toolchains/aarch64-elf-5.3.0-Darwin-x86_64/bin
export PATH=$PATH:$SRC/toolchains/x86_64-elf-5.3.0-Darwin-x86_64/bin
```
--->
```
# 对于Linux
export PATH=$PATH:$SRC/toolchains/aarch64-elf-5.3.0-Linux-x86_64/bin
export PATH=$PATH:$SRC/toolchains/x86_64-elf-5.3.0-Linux-x86_64/bin

# 对于Mac
export PATH=$PATH:$SRC/toolchains/aarch64-elf-5.3.0-Darwin-x86_64/bin
export PATH=$PATH:$SRC/toolchains/x86_64-elf-5.3.0-Darwin-x86_64/bin
```

<!---
## Copying files to and from Zircon

With local link IPv6 configured, the host tool ./build-ARCH/tools/netcp
can be used to copy files.
--->
## Zircon双向拷贝文件

若本地IPv6网络配置成功，便可以使用主机工具`./build-zircon-ARCH/tools/netcp`来拷贝文件。

<!---
```
# Copy the file myprogram to Zircon
netcp myprogram :/tmp/myprogram

# Copy the file myprogram back to the host
netcp :/tmp/myprogram myprogram
```
--->
```
# 拷贝myprogram文件到Zircon
netcp myprogram :/tmp/myprogram

# 拷贝Zircon的myprogram文件到开发主机
netcp :/tmp/myprogram myprogram
```

<!---
## Including Additional Userspace Files
--->
## 添加额外的用户态文件

<!---
The Zircon build creates a bootfs image containing necessary userspace components
for the system to boot (the device manager, some device drivers, etc).  The kernel
is capable of including a second bootfs image which is provided by QEMU or the
bootloader as a ramdisk image.
--->
Zircon的构建过程会产生一个bootfs镜像，它包含系统启动必需的用户态组件（包括设备管理器，一些设备驱动等）。除此之外，内核也能够包含一个以ramdisk镜像的形式，由QEMU或者bootloader提供的额外镜像。

<!---
To create such a bootfs image, use the mkbootfs tool that's generated as part of
the build.  It can assemble a bootfs image for either source directories (in which
case every file in the specified directory and its subdirectories are included) or
via a manifest file which specifies on a file-by-file basis which files to include.
--->
为了产生该bootfs镜像，请使用在构建中同时生成的`mkbootfs`工具。`mkbootfs`可以通过两种方式装配出一个bootfs镜像：通过目标目录（该目录的所有文件和子目录都将包含在内）或通过逐行列出需要包括在内的清单文件：
<!---
```
$BUILDDIR/tools/mkbootfs -o extra.bootfs @/path/to/directory

echo "issue.txt=/etc/issue" > manifest
echo "etc/hosts=/etc/hosts" >> manifest
$BUILDDIR/tools/mkbootfs -o extra.bootfs manifest
```
--->
```
$BUILDDIR/tools/mkbootfs -o extra.bootfs @/path/to/directory

echo "issue.txt=/etc/issue" > manifest
echo "etc/hosts=/etc/hosts" >> manifest
$BUILDDIR/tools/mkbootfs -o extra.bootfs manifest
```
<!---
On the booted Zircon system, the files in the bootfs will appear under /boot, so
in the above manifest example, the "hosts" file would appear at /boot/etc/hosts.
--->
在Zircon系统启动完成后，bootfs镜像中的文件将出现在`/boot`目录下，所以上面例子中的"host"文件位于`/boot/etc/hosts`。

<!---
For QEMU, use the -x option to the run-zircon-* scripts to specify an extra bootfs image.
--->
对于QEMU，请使用`run-zircon-*`脚本的-x选项来指定额外的bootfs镜像。

<!---
## Network Booting
--->
## 网络启动

<!---
Network booting is supported via two mechanisms: Gigaboot and Zirconboot.
Gigaboot is an EFI based bootloader whereas zirconboot is a mechanism that
allows a minimal zircon system to serve as a bootloader for zircon.
--->
有两种机制支持网络启动Zircon：Gigaboot和Zirconboot。Gigaboot是基于EFI的bootloader，而Zirconboot则是允许一个最小化的zircon系统来充当启动zircon自身的bootloader。

<!---
On systems that boot via EFI (such as Acer and NUC), either option is viable.
On other systems, zirconboot may be the only option for network booting.
--->
在支持通过EFI启动的硬件设备（如Acer和NUC）上，上述两种方式都是支持的。而对于其他系统，zirconboot可能是网络启动的唯一选项。

<!---
### Via Gigaboot
The [GigaBoot20x6](https://fuchsia.googlesource.com/zircon/+/master/bootloader) bootloader speaks a simple network boot protocol (over IPV6 UDP)
which does not require any special host configuration or privileged access to use.
--->
### 通过Gigaboot启动
基于[GigaBoot20x6（英文原文）](https://github.com/fuchsia-mirror/zircon/tree/master/bootloader)的bootloader使用一种简单的基于IPV6的UDP网络启动协议，它不需要特殊的服务器配置和使用权限设置。

<!---
It does this by taking advantage of IPV6 Link Local Addressing and Multicast,
allowing the device being booted to advertise its bootability and the host to find
it and send a system image to it.
--->
它利用IPV6链路的本地寻址和广播来达到此目的，允许目标设备发布它的启动消息，开发主机在接收消息后发送启动镜像到目标设备。

<!---
If you have a device (for example a Broadwell or Skylake Intel NUC) running
GigaBoot20x6 first create a USB drive [manually](https://fuchsia.googlesource.com/zircon/+/master/docs/targets/acer12.md#How-to-Create-a-Bootable-USB-Flash-Drive)
or (Linux only) using the [script](https://fuchsia.googlesource.com/scripts/+/master/build-bootable-usb-gigaboot.sh).
--->
如果你有运行GigaBoot20x6的硬件设备（例如配备Broadwell或Skylake结构的CPU的Intel NUC），请首先[手动创建USB启动盘（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/targets/acer12.md#how-to-create-a-bootable-usb-flash-drive)，或使用[脚本](https://github.com/fuchsia-mirror/scripts/blob/master/build-bootable-usb-gigaboot.sh)（仅针对Linux）。然后运行：
<!---
```
$BUILDDIR/tools/bootserver $BUILDDIR/zircon.bin

# if you have an extra bootfs image (see above):
$BUILDDIR/tools/bootserver $BUILDDIR/zircon.bin /path/to/extra.bootfs
```
--->
```
$BUILDDIR/tools/bootserver $BUILDDIR/zircon.bin

# 如果有额外的bootfs镜像（见上）：
$BUILDDIR/tools/bootserver $BUILDDIR/zircon.bin /path/to/extra.bootfs
```

<!---
By default bootserver will continue to run and every time it obsveres a netboot
beacon it will send the kernel (and bootfs if provided) to that device.  If you
pass the -1 option, bootserver will exit after a successful boot instead.
--->
引导服务器默认将一直运行，一旦检测出有网络启动的请求，它就会将内核（包括bootfs，如果有的话）发送到该请求设备上。如果在启动引导服务时传递-1选项，那么它将在执行一次成功的启动后停止服务并退出。

<!---
### Via Zirconboot
Zirconboot is a mechanism that allows a zircon system to serve as the
bootloader for zircon itself. Zirconboot speaks the same boot protocol as
Gigaboot described above.
--->
### 通过Zirconboot启动

Zirconboot是一种允许Zircon系统充当启动Zircon自身的bootloader机制，而且Zirconboot使用和前文提到的Gigaboot相同的启动协议。

<!---
To use zirconboot, pass the `netsvc.netboot=true` argument to zircon via the
kernel command line. When zirconboot starts, it will attempt to fetch and boot
into a zircon system from a bootserver running on the attached host.
--->
为了使用Zirconboot，请通过在kernel命令行种传递`netsvc.netboot=true`选项到Zircon的方式。当Zirconboot启动时，它将试图从挂载于开发主机的引导服务器上获取并启动zircon系统。

<!---
## Network Log Viewing

The default build of Zircon includes a network log service that multicasts the
system log over the link local IPv6 UDP.  Please note that this is a quick hack
and the protocol will certainly change at some point.
--->
## 通过网络查看日志

Zircon的构建过程默认包含了网络日志服务，它通过本地IPv6的UDP协议广播系统日志。请注意，这只是一种快速能用的方式，未来某个时刻协议肯定会发生改变。

<!---
For now, if you're running Zircon on QEMU with the -N flag or running on hardware
with a supported ethernet interface (ASIX USB Dongle or Intel Ethernet on NUC),
the loglistener tool will observe logs broadcast over the local link:
--->
当前，如果你在QEMU上加-N选项运行zircon或者在配备以太网卡的真实硬件上（ASIX上的USB网络适配器或NUC上的Intel网卡），loglistener工具可以通过本地网络接收日志广播：

<!---
```
$BUILDDIR/tools/loglistener
```
--->
```
$BUILDDIR/tools/loglistener
```

<!---
## Debugging

For random tips on debugging in the zircon environment see
[debugging](debugging/tips.md).
--->
## 调试

关于在Zircon环境中进行调试的随机提示信息，请查看[调试（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/debugging/tips.md)部分。

<!---
## Contribute changes
* See [contributing.md](contributing.md).
--->
## 贡献代码变更
* 请查看[贡献代码（英文原文）](https://github.com/fuchsia-mirror/zircon/blob/master/docs/contributing.md)部分。

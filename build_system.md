# Fuchsia构建系统

该文档描述了使用`gen.py`脚本和`ninja`工具进行直接构建，以及使用`scripts/run-zircon-*`启动QEMU的方法。

除此之外，你也可以使用[标准构建方法](getting_started.md#构建fuchsia)中的`fx`命令来进行构建。

### 构建Zircon和sysroot

在构建Fuchsia之前，你需要首先[构建Zircon内核](https://github.com/fuchsia-mirror/zircon/blob/master/docs/getting_started.md)和sysroot。

```
(cd zircon; make -j32 zircon-pc-x86-64)
./scripts/build-zircon.sh
```

### 构建Fuchsia

使用下面的命令构建Fuchsia：

```
./packages/gn/gen.py
./buildtools/ninja -C out/debug-x86-64
```

如果系统安装和配置了ccache（即CCACHE_DIR环境变量被指定到一个已存在的目录），可以使用下面的命令进行快速构建：

```
./packages/gn/gen.py --ccache
./buildtools/ninja -C out/debug-x86-64
```

可以向gen.py脚本传递选项`--target_cpu`来指定目标体系结构，如果未指定，则默认是x86-64。

```
./packages/gn/gen.py --target_cpu=aarch64
./buildtools/ninja -C out/debug-aarch64
```

通过向`--packages`传递应用包（package）列表来配置需要构建的package。并且在运行一次`gen.py`之后，以后即可使用`ninja`来进行增量构建。

你还可以通过向`--autorun`传递一个脚本的路径，来实现该脚本的“自动运行”。例如，下面的示例演示了在系统启动后立即关机的效果。

```
echo 'dm poweroff' > poweroff.autorun
./packages/gn/gen.py --autorun=poweroff.autorun
./buildtools/ninja -C out/debug-x86-64
```

另外，运行`gen.py --help`可获取`gen.py`完整的选项列表。

### 运行Fuchsia

上一步的运行生成了名为`out/debug-{arch}/user.bootfs`的文件系统文件。为了在QEMU中挂载该文件系统，需在Zircon的启动脚本中通过`-x`选项传递该文件路径，例如：

```
./scripts/run-zircon-x86-64 -x out/debug-x86-64/user.bootfs -m 2048
./scripts/run-zircon-arm64 -x out/debug-aarch64/user.bootfs -m 2048
```

对于可传递到QEMU的其他选项，请查看[标准构建方法](getting_started.md#从qemu启动)

### 在ARM硬件上运行

为了构建针对特定ARM硬件的Fuchsia镜像，需在`gen.py`中通过向zircon_project参数传递该硬件目标的项目名。例如，为了构建针对树莓派3的Fuchsia镜像，可以采用如下方式调用gen.py：

```
./packages/gn/gen.py --zircon_project=zircon-rpi3-arm64 --target_cpu=aarch64
./buildtools/ninja -C out/debug-aarch64
```

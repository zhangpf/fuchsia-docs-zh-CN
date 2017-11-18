Fuchsia源码
==============

Fuchsia使用`jiri`工具来管理git仓库（
[https://github.com/fuchsia-mirror/jiri](https://github.com/fuchsia-mirror/jiri)），该工具可以通过清单描述文件（manifest）来管理一组git仓库。

关于如何构建源码，请查看[入门](getting_started.md)文档。

## 创建工作目录（和下载源码）

该初始化过程需要系统已经安装了Go（版本为1.6或者更新）和git，并且已经加入到PATH环境变量中。

首先，选择你想要构建的[layer](layers.md)（如果不知道如何选择，那么请选择`topaz`，它也同时包含了它下面的所有layer），然后运行如下命令：

```
curl -s "https://fuchsia.googlesource.com/scripts/+/master/bootstrap?format=TEXT" | base64 --decode | bash -s <layer>
```

该命令将初始化一个以该layer命名的目录作为开发环境，如果脚本运行成功，将会在最后打印一段消息，内容是建议你将`.jiri_root/bin`加入到系统的PATH变量中

### 不改变PATH变量的工作方式

如果你不想改变你的环境变量，而且`jiri`能根据你的当前工作目录，达到“够用就好”的目的，那么只需将`jiri`拷贝到PATH中的某个目录下。但是请注意，**你必须对该目录具有写权限（**而不是通过sudo的方式），否则，`jiri`将不能更新它自己到最新状态。

```
cp .jiri_root/bin/jiri ~/bin
```

为了使用`fx`工具，你也可以选择创建符号链接到`~/bin`目录：

```
ln -s `pwd`/scripts/fx ~/bin
```

或者采用直接运行`scripts/fx`的方式。

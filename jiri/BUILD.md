# 构建Jiri
---

[*英文原文快照*](https://github.com/fuchsia-mirror/jiri/blob/6ddcc1e3e9d9c4aba2eb0446b5b1345924c823af/BUILD.md)

---

## 前提条件

* cmake 3.7.2
* golang 1.7.3
* ninja 1.7.2
* git 2.7.4

## 获取代码

### 使用jiri的预构建版本

该方法只适用于Linux（x86\_64和aarch64架构）和darwin（x86\_64）系统。该自举过程要求系统安装Go 1.6或更新版本，已经安装好Git，并在`PATH`环境变量上。下面的命令将在名为`fuchsia`的新目录中创建checkout。

This method only works with linux (x86\_64 and aarch64 ) and darwin (x86\_64) systems.
The bootstrap procedure requires that you have Go 1.6 or newer and Git installed and on your `PATH`. Below command will create checkout in new folder called `fuchsia`.
```
curl -s https://raw.githubusercontent.com/fuchsia-mirror/jiri/master/scripts/bootstrap_jiri | bash -s fuchsia
cd fuchsia
export PATH=`pwd`/.jiri_root/bin:$PATH
jiri import jiri https://fuchsia.googlesource.com/manifest
jiri update
```
### 手动
创建名为`fuchsia`的根目录，然后使用git来手动克隆在该[清单](jiri manifest)中提到的每个项目，将它们置于正确的路径上并按照正确的版本进行checkout，在清单中没有指定版本号时，`HEAD`应置于`origin/master`。
Create a root folder called `fuchsia`, then use git to manually clone each of the projects mentioned in this [manifest][jiri manifest], put them in correct paths and checkout required revisions. `HEAD` should be on `origin/master` where no revision is mentioned in manifest.

## 构建
设置GOPATH环境变量为`fuchsia/go`，cd到`fuchsia/go/src/fuchsia.googlesource.com/jiri`目录然后运行
```
./scripts/build.sh
```

The above command should build jiri and put it into your jiri repo root.
上面的命令构建jiri并把构建的结果放入jiri仓库的根目录下。

## 运行测试
为了运行jiri的测试，在`fuchsia/go`目录下运行如下命令：
```
export GOPATH=$(pwd)
go test $(go list fuchsia.googlesource.com/jiri/... 2>/dev/null | grep -v /jiri/vendor/)
```

(The use of `grep` here excludes tests from packages below `src/fuchsia.googlesource.com/jiri/vendor/` which don't pass.)

## 已知问题

如果构建过程报出未定义`http_parser_ *`函数的错误，请从库路径中移除`http_parser`。

If build complains about undefined `http_parser_*` functions, please remove `http_parser` from your library path.

[jiri manifest]: https://github.com/fuchsia-mirror/manifest/blob/master/jiri "jiri manifest"

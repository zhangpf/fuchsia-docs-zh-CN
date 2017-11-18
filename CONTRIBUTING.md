贡献代码变更
====================

Fuchsia通过[Gerrit](https://fuchsia-review.googlesource.com)来管理代码提交。需要说明的是并非所有的子项目都接收补丁（patch），具体详情请查看每个子项目的CONTRIBUTING.md文档。


## 提交代码变更

为了向Fuchsia提交patch，需要首先生成授权你到Gerrit的cookie项。为此，请首先登录到Gerrit，然后点击位于[https://fuchsia.googlesource.com](https://fuchsia.googlesource.com)顶部的“Generate Password”的链接。然后拷贝生成的命令到终端并执行。

一旦授权成功，便可通过以下的步骤向Fuchsia的某个子项目提交patch。

```
# 创建新的分支
git checkout -b branch_name

# 修改代码, 在该分支提交代码
# 编辑some_file……
git add some_file
# 请遵循该子项目规定的提交描述消息的格式（如果有的话）
git commit ...

# 向Gerrit上传patch
# `jiri help upload`可以列出各种功能选项。例如，增加代码审查者等
jiri upload # 使用默认的话题（topic） - ${USER}-branch_name
# 或者
jiri upload -topic="custom_topic"
# 或者
git push origin HEAD:refs/for/master

# 此时如果你还想编辑你的patch，请使用`--amend`选项
git commit --amend

# 一旦上传成功，请删除该分支
git branch -d branch_name
```

更多细节请查看Gerrit的文档：
[https://gerrit-documentation.storage.googleapis.com/Documentation/2.12.3/intro-user.html#upload-change](https://gerrit-documentation.storage.googleapis.com/Documentation/2.12.3/intro-user.html#upload-change)

## [仅针对非Googlers] 签署Google贡献者许可证协议（CLA）

在上传代码变更之前，请首先签署Google贡献者许可证协议（[Google CLA](https://cla.developers.google.com/))。

## 在层之间切换

在初始化开发环境（详情请查看[获取代码](getting_source.md)）步骤中，你已选择了一个层（layer）进行开发。该层的代码处于最新版本，而它下面的层则处于过去的某个历史版本中（*译者注：类似于git中的submodule，子模块并不会直接更新到最新版本*）。

如果你想切换到其他层，要么在代码树中获取更高层的源代码，要么当前查看的是更低层的的某个历史版本，对此你有两个选择：

1. 采用和之前一样初始化的方式，[为该层初始化新的开发环境](getting_source.md)。
2. 通过`fx set-layer <layer>`命令切换现有的开发环境。该命令通过改变代码树`jiri`元信息来指向新的层，并显示如何真正获取代码和构建新配置层的说明。

## 跨层的代码变更

因为Fuchsia被分成多[层](layers.md)，每层看到的它下面的层其实是被固定到某个特定的历史版本，也就是说一个层更新后的代码并不会立即被上方的层看到。

当创建层的变更时，你需要考虑清楚在何时不同层会看到变更的不同部分，例如，当你想改变Zircon的接口，并且影响到Gernet的上层客户代码时，其他开发者在构建Gernet时并不会立刻看到Zircon的提交。相反，只有在Gernet更新了它指向Zricon的版本时才会看到这些更新。

### 软变换（推荐）

对于跨层变更的一种较好方式是使用*软变换*。在这种方式下，你提交到较低层（例如Zircon）的代码变更会同时支持新旧两种接口使用方式。例如，如果你想替换某个函数，你可以增加该函数的新版本，并用新版本来封装旧版本。

采用如下的步骤来进行软变换：

1. 提交较低层层（例如Zircon）的代码变更，在不破坏上层（例如Garnet）使用的旧接口前提下，引入新的接口；
2. 等待自动回滚bot更新上层指向的下层的版本号；
3. 提交更改上层的代码，迁移到新的接口；
4. 提交底层的变更，删掉旧接口。

### 硬变换

对于某些变更，创建软变换是非常困难或者根本不可行的，对此，你可以使用*硬变换*方式。在这种方式下，你创建破坏性变更来更新底层代码，并手动更新上层代码。

采用如下的步骤来进行硬变换：

1. 上传底层（例如Zircon）破坏上层使用接口的变更。此时，自动回滚bot的更新上层操作将会失败；
2. 上传上层的变更，迁移到新的接口，以及通过编辑上层的`//<layer>/manifest/<layer>` 清单文件中底层的`revision`属性来更新指向底层的版本号。

进行硬变换比软变换会产生更大的压力，因为在步骤1和步骤2之间将会阻止其他代码变更的发生。

## 跨仓库变更

对于两个或多个不同的子项目仓库的变更，如果你使用了相同的话题，Gerrit会为你自动进行跟踪。

### 使用jiri进行上传

在所有的子项目上创建相同名字的分支，然后提交变更
```
# 创建和提交第一个变更
cd fuchsia/bin/fortune
git checkout -b add_feature_foo
*编辑foo_related_files……*
git add foo_related_files ...
git commit ...

# 在另一个子项目中创建和提交第一个变更
cd fuchsia/build
git checkout -b add_feature_foo
*编辑edit more_foo_related_files……*
git add more_foo_related_files ...
git commit ...

# 跨多个项目使用项目分支名上传变更Upload all changes with the same branch name across repos
jiri upload -multipart # 使用默认的话题 - ${USER}-branch_name
# 或者
jiri upload -multipart -topic="custom_topic"

# 当变更审查完成，通过且合并后，删除本地的临时分支
cd fuchsia/bin/fortune
git branch -d add_feature_foo

cd fuchsia/build
git branch -d add_feature_foo
```

### 使用Gerrit的命令

```
# 使用名为'add_feature_foo'的话题，创建和提交第一个变更
cd fuchsia/bin/fortune
git checkout -b add_feature_foo
*编辑foo_related_files……*
git add foo_related_files ...
git commit ...
git push origin HEAD:refs/for/master%topic=add_feature_foo

# 在另一个子项目中创建和提交第一个变更
cd fuchsia/build
git checkout -b add_feature_foo
*编辑more_foo_related_files……*
git add more_foo_related_files ...
git commit ...
git push origin HEAD:refs/for/master%topic=add_feature_foo

# 当审查完成，通过且合并变更后，删除本地的临时分支
cd fuchsia/bin/fortune
git branch -d add_feature_foo

cd fuchsia/build
git branch -d add_feature_foo
```

变更的多个部分在Gerrit中通过相同话题进行追踪，并且也会一起进行测试，也可以通过`Submit Whole Topic`向Gerrit提交它们。另外，还可以通过网页编辑话题。

## 消解合并冲突

```
# 在origin/master进行rebase操作， 显示合并冲突
git rebase origin/master

# 消解这些冲突，并且完成rebase
*编辑files_with_conflicts……*
git add files_with_resolved_conflicts ...
git rebase --continue
jiri upload

# 完成后再进行正常的操作
git commit --amend
jiri upload
```

## Github集成

除了部署于[https://fuchsia.googlesource.com](https://fuchsia.googlesource.com)的Fuchsia代码之外，同时在Gihub也有Fuchsia的镜像[https://github.com/fuchsia-mirror](https://github.com/fuchsia-mirror)。为了保证Fuchsia的贡献同时也关联到你的Github账号：

1. [在git中设置好邮箱](https://help.github.com/articles/setting-your-email-in-git/)；
2. [将你的邮箱地址加入到Github账号中](https://help.github.com/articles/adding-an-email-address-to-your-github-account/)；
3. 为了使你的贡献出现在主页的贡献活动（Contribution Activity）中，请标星该子项目。

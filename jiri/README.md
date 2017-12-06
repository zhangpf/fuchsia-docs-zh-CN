# Jiri

*"Jiri integrates repositories intelligently"*

Jiri是面向多仓库（multi-repo）开发的工具：
它支持：

* 同步多个本地代码仓库
* 向所有本地仓库的当前状态捕获快照
* 从快照中恢复本地项目状态，以及
* 很方便地向[Gerrit][gerrit]发送代码变更列表。

Jiri具有可扩展的插件模型，可以很容易地创建新的子命令。
Jiri是开源的。

## 手动构建jiri
我们有linux和darwin的`x86_64`系统的[预构建文件](#自举)。而为了手动构建jiri，请使用[该指南][build jiri]。

## Jiri的行为
[请查看][behaviour]

## Jiri的技巧
[请查看][how do i]

## Jiri基础

Jiri根据清单（manifest）内容在本地文件系统中组织一组代码仓库，这些代码仓库库被称为“项目（project）”，而且都包含在一个名为“jiri root”的目录中。

清单文件指定每个项目在jiri root中的相对位置，并还包括有关这些项目的其他元数据，例如其远程URL，它跟踪的远程分支等。

`jiri update`命令将所有本地项目的主分支同步到每个项目的清单中指定的版本和远程分支的状态，jiri会在本地项目不存在的情况下在本地创建项目，如果使用`-gc`选项运行`jiri update`，jiri将删除清单中没有列出但已存在的本地项目。

jiri root中的`.jiri_manifest`文件描述了需要jiri同步的项目，通常`.jiri_manifest`文件将导入其他清单文件，但它也可以包含项目的列表。

例如，下面是一个仅包含两个项目的简单`.jiri_manifest`文件，这两个项目是“foo”和“bar”，分别部署于github和bitbucket上。
```
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <projects>
    <project name="foo-project"
             remote="https://github.com/my-org/foo"
             path="foo"/>
    <project name="bar"
             remote="https://bitbucket.com/other-org/bar"
             path="bar"/>
  </projects>
</manifest>
```
当你首次运行`jiri update`时，“foo”和“bar”仓库将分别被克隆至`foo`和`bar`目录下，并且被置于DETACHED HEAD状态。再次运行`jiri update`将会更新所有的远程引用，并将你当前的分支rebase到它的上游分支上。

请注意，项目的路径不必是jiri root的直接子目录，而是可以自由决定将“bar”项目的path属性设置为“third_party/bar”，甚至可以通过将“bar”项目的`path`设置为“foo/bar”，以将其嵌套在“foo”中（假设foo仓库中没有文件与bar仓库冲突）。

由于清单文件也需要在团队各个成员之间保持同步，因此将团队的清单保存在版本控制仓库中通常是有意义的。

使用jiri的`jiri import`命令，可使本地`.jiri_manifest`文件导入远程清单变得容易。例如，运行下面的命令将创建一个`.jiri_manifest`文件（或附加到现有`.jiri_manifest`文件中），文件中的一个`import`标签指定从`https//fuchsia.googlesource.com/manifest`仓库中导入名为minimal的清单。

当下次运行`jiri update`时，jiri将同步minimal清单中列出的所有项目到本地。

## 快速开始

本节介绍如何上手使用jiri。

我们首先需“自举”jiri，以便它可以同步和构建自己。

而后我们创建并导入一个新的清单，它指定了jiri应该如何管理你的项目。

### 自举

你可以使用[自举脚本][bootstrap_jiri]立即获取jiri。

首先，选择一个jiri的根目录，而后所有的项目将会被同步到它的子目录下。
```
export MY_ROOT=$HOME/myroot
```
执行`jiri_bootstrap`脚本，它将获取和构建jiri工具，并初始化根目录。
```
curl -s https://raw.githubusercontent.com/fuchsia-mirror/jiri/master/scripts/bootstrap_jiri | bash -s "$MY_ROOT"
```
`jiri`命令行工具将被安装到`$MY_ROOT/.jiri_root/bin/jiri`目录下，将该目录添加到`PATH`中。
```
export PATH="$MY_ROOT"/.jiri_root/bin:$PATH
```
然后，使用`jiri import`命令从Fuchsia仓库中导入minimal清单，该清单仅包含能支撑jiri工作的项目。

你可以在[这里][minimal manifest]查看minimal清单。对于清单文件更多的信息，请阅读[文档][manifests]。
```
cd "$MY_ROOT"
jiri import minimal https://fuchsia.googlesource.com/manifest
```
现在，在jiri根目录下已有名为`.jiri_manifest`的文件，该文件仅包含唯一的import标签。
最后，运行 `jiri update`，它将所有的本地项目同步到清单中列出的版本状态下（在我们的例子里是HEAD状态）。
```
jiri update
```
现在，你可以在`$MY_ROOT/manifest`目录下看到导入的项目。

再次运行`jiri update`将同步本地仓库到远程状态，并同时更新jiri工具。

### 用jiri管理你的项目

现在jiri已经能够同步和构建它自身了，但我们还需向其指定如何管理我们的项目。

为了使jiri能管理一组项目，这些项目需要在[清单文件][manifests]中列出, 并且该清单文件也必须被托管在git仓库中。

如果已经有托管在git仓库的清单，可采用与导入“minimal”同样的方式导入该清单文件。

例如，如果你有名为“my_manifest”的清单托管于[https://github.com/my_org/manifests](https://github.com/my_org/manifests)仓库中，那么你可以按如下方式导入该清单：
```
jiri import my_manifest https://github.com/my_org/manifests
```
本节的其余部分将介绍如何从头开始创建清单，以及在本地git仓库托管并用jiri来管理它。

假设你需要jiri管理的项目是“Hello-World”仓库，它位于https://github.com/Test-Octowin/Hello-World。

首先，我们创建一个新的git仓库来托管将要写的清单。
```
mkdir -p /tmp/my_manifest_repo
cd /tmp/my_manifest_repo
git init
```
然后，我们创建新的清单，并提交至manifest仓库中。

该清单文件同时包含Hello-World仓库，以及mainfest仓库本身。
```
cat <<EOF > my_manifest
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <projects>
    <project name="Hello-World"
             remote="https://github.com/Test-Octowin/Hello-World"
             path="helloworld"/>
    <project name="my_manifest_repo"
             remote="/tmp/my_manifest_repo"
             path="my_manifest_repo"/>
  </projects>
</manifest>
EOF

git add my_manifest
git commit -m "Add my_manifest."
```
该清单包含唯一一个名为Hello-World的项目以及追踪的远程仓库，`path`属性告诉jiri在`helloworld`目录下同步该仓库。

通常情况下，我们希望将该仓库push到远程服务器上，以便其他用户同步该项目。 但是，现在我们还只能通过本地文件系统中的路径来引用该仓库。

现在需要导入新的清单，并执行`jiri update`。
```
cd "$MY_ROOT"
jiri import my_manifest /tmp/my_manifest_repo
jiri update
```
你可以在`$MY_ROOT/helloworld`目录下看到Hello-World仓库，以及在`$MY_ROOT/my_manifest_repo`目录下的清单文件仓库。

## 命令行帮助

`jiri help`命令将显示关于`jiri`工具和其子命令的帮助文档。

对于全面的文档，包括子命令列表等，请运行`jiri help`。要查找有关特定主题或子命令的文档，请运行`jiri help <command>`。

### 主要的子命令包括：
```
   branch          显示和删除分支
   diff            显示两份快照的差异
   grep            跨项目搜索
   import          向.jiri_manifest导入项目
   init            创建新的jiri root
   patch           现有项目的补丁
   project         管理jiri列出的项目
   project-config  打印/设置项目的本地配置
   run-hooks       使用本地清单运行hooks
   runp            跨jiri项目地并行运行命令
   selfupdate      更新jiri工具
   snapshot        创建新的项目快照
   source-manifest 从当前的checkout创建一个新的源代码清单
   status          打印所有项目的当前状态
   update          更新所有的jiri项目
   upload          上传变更列表以便于代码评审
   version         打印jiri的版本
   help            显示某个子命令或主题的帮助文档
```
运行`jiri help [command]`以获取命令的用法。

## 文件系统

查看jiri[文件系统文档][filesystem doc]。

## 清单

查看jiri[清单文档][manifest doc]。

## 快照

TODO

## Gerrit CL工作流

[Gerrit][gerrit]是被许多开源项目使用的协作代码评审工具。

Gerrit的特点之一是它认为一个变更列表仅由一个提交表示，因此这限制了开发人员使用git来处理变更的方式。特别地，他们必须使用--amend标志来进行除了第一个之外的其他git提交操作，并且需要使用git rebase来与远程主机同步他们待定（pending）状态的代码变更。请参阅Android的[repo命令参考][android repo]或Go的[贡献说明][go contrib]作为例子，来了解消解待定代码变更与远程主服务器之间冲突的工作流，是如何的错综复杂。

本节的其余部分将介绍使用`jiri upload`的常见开发操作。

### 使用功能分支

所有的开发都应该在一个非master的“功能”分支上进行。一旦代码审核通过后，通过Gerrit代码评审系统将其合并到远程master分支中，而后可以使用`jiri update -rebase-all`将变更合并到本地分支。

### 创建新的CL

1. 同步到master分支到远程主机状态。
  ```
  jiri update
  ```
2. 为CL创建新的功能分支。
  ```
  git checkout -b <branch-name> --track origin/master
  ```
3. 对项目源代码进行修改。
4. 将任何更改过的文件暂存以便提交。
  ```
  git add <file1> <file2> ... <fileN>
  ```
5. 提交变更。
  ```
  git commit
  ```

### 与远程同步CL

1. 将master分支与远程同步
  ```
  jiri update
  ```
2. 切换到CL对应的正在开发中的功能分支上。
  ```
  git checkout <branch-name>
  ```
3. 将功能分支与master分支同步。
  ```
  git rebase origin/master
  ```
4. 如果master分支和功能分支之间没有冲突，则CL已经与远程成功同步。
5. 如果存在冲突：
  1. 手动[消解冲突][github resolve conflict]。
  2. 暂存所有变更的文件作为一次提交。
    ```
    git add <file1> <file2> ... <fileN>
    ```
  3. 提交该变更。
    ```
    git commit --amend
    ```

### 请求代码评审

1. 切换到CL对应的正在开发中的功能分支上。
  ```
  git checkout <branch-name>
  ```
2. 上传该CL到Gerrit。
  ```
  jiri upload
  ```
  
如果上传CL成功，则会打印托管在Gerrit上CL的URL地址，您可以通过该URL的[Gerrit的Web界面][gerrit web ui]添加审阅者和评论。

请注意，`jiri upload`有许多有用的标志选项，你可以通过运行`jiri help upload`了解他们。

### 评审CL

1. 依照代码评审请求邮件中收到的链接。
2. 使用[Gerrit的Web界面][gerrit web ui]在CL上进行评论并点击“Reply”按钮提交评论，选择合适的代码评审分数。


### 处理评审评论

1. 换到CL对应的正在开发中的功能分支上。
  ```
  git checkout <branch-name>
  ```
2. 更改和评论代码
  ```
  git commit --amend
  ```
3. 回复每个Gerrit评论，然后单击“Reply”按钮发送它们。
4. 发送上传的CL到Gerrit
  ```
  jiri upload
  ```

### 提交CL
1. 请注意，如果CL与自CL上次更新以来提交的任何代码变更发生冲突，则需要在提交CL之前解决这些冲突。为此，rebase你的变更，然后将变更的CL上传到Gerrit。
2. 一旦CL符合提交条件，可以通过单击Gerrit Web界面上的“Submit”按钮将其合并到远程master分支上。
3. 在CL提交至Gerrir之后，删除本地功能分支。
  1. 将master分支同步到远程的最新版本上。
    ```
    jiri update
    ```
  2. 安全删除与CL对应的功能分支。
    ```
    git checkout JIRI_HEAD && git branch -d <branch-name>
    ```

### CL依赖项

如果你同时有变更A和B，而B依赖于A，你仍然可以为A和B提交不同的的CL，并进行独立评审和提交（虽然A必须在B之前提交）。

首先，为A创建您的功能分支，进行你的代码变更，然后根据上面的说明上传CL进行代码评审。

然后，在当前仍然处于A的特征分支上时，为B创建你的特征分支。
```
git checkout -b feature-B --track origin/master
```
然后根据前面的说明，进行你的代码变更和上传CL进行代码评审。

您依然可以通过通常的方式提交新的补丁集来回复评论。

在A的CL提交之后，确保清理A的特性分支并为功能B上传新的补丁集。
```
jiri update # 获取包含功能A的更新
git checkout feature-B
git rebase -i origin/master # 如果你看到功能A的提交，删除它，然后正确地进行rebase
jiri upload # 发送功能B的新补丁集
```
功能B的CL现在可以进行提交。
以上过程可以扩展到2个以上的CL，但你必须牢记两件事：

* 总是在父特征分支上创建子特征分支，以及
* 在提交父功能之后，将功能B的分支rebase到origin/master之上

## FAQ

### 为什么要使用“jiri”这个名字？

[Jiří][jiri-wiki]在捷克是一个非常受欢迎的男孩名字。

### “jiri”如何发音？
我们把“jiri”发音像“yiree”。

而实际上捷克名字[Jiří][jiri-wiki]的发音却像“yirzhee”。

### 如何在不向上游push代码情况下测试清单的变更
请查看[Jiri的本地更新][hacking doc]

[android repo]: https://source.android.com/source/using-repo.html "Repo command reference"
[bootstrap_jiri]: https://github.com/fuchsia-mirror/jiri/blob/master/scripts/bootstrap_jiri "bootstrap_jiri"
[gerrit]: https://code.google.com/p/gerrit/ "Gerrit code review"
[gerrit web ui]: https://gerrit-review.googlesource.com/Documentation/user-review-ui.html "Gerrit review UI"
[github resolve conflict]: https://help.github.com/articles/resolving-a-merge-conflict-from-the-command-line/ "Resolving a merge conflict"
[go contrib]: https://golang.org/doc/contribute.html#Code_review "Go Contribution Guidelines - Code Review"
[jiri-wiki]: https://en.wikipedia.org/wiki/Ji%C5%99%C3%AD "Jiří"
[manifests]: #清单 "manifests"
[minimal manifest]: https://github.com/fuchsia-mirror/manifest/blob/master/minimal "minimal manifest"
[manifest doc]: /jiri/manifest.md "Jiri manifest"
[filesystem doc]: /jiri/filesystem.md "Jiri filesystem"
[hacking doc]: /jiri/HACKING.md "Jiri local updates"
[behaviour]: /jiri/behaviour.md
[build jiri]: /jiri/BUILD.md "Build jiri"
[how do i]: /jiri/howdoi.md

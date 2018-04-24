# Jiri清单
---

[*英文原文快照*](https://github.com/fuchsia-mirror/jiri/blob/6ddcc1e3e9d9c4aba2eb0446b5b1345924c823af/manifest.md)

---

Jiri清单文件描述了运行“jiri update”时发生同步的项目集合。

jiri读取的第一个清单文件是[root]/.jiri\_manifest，为使jiri工具能够正常工作，该清单**必须**存在。

[root]/.jiri\_manifest清单通常会用<import>标签的方式从远程仓库中导入其他的清单文件，但它也可以包含自己的项目列表。

清单具有以下XML模式：
```
<manifest>
  <imports>
    <import remote="https://vanadium.googlesource.com/manifest"
            manifest="public"
            name="manifest"
    />
    <localimport file="/path/to/local/manifest"/>
    ...
  </imports>
  <projects>
    <project name="my-project"
             path="path/where/project/lives"
             protocol="git"
             remote="https://github.com/myorg/foo"
             revision="ed42c05d8688ab23"
             remotebranch="my-branch"
             gerrithost="https://myorg-review.googlesource.com"
             githooks="path/to/githooks-dir"
    />
    ...
  </projects>
  <hooks>
    <hook name="update"
          project="mojo/public"
          action="update.sh"/>
    ...
  </hooks>

</manifest>
```
`<import>`和`<localimport>`标签可用于跨清单共享公共的项目。

当被导入的清单和执行导入清单都在同一个代码库中，或者两者都不在代码库中时，应该使用`<localimport>`标签。 “file”属性是被导入的清单文件的路径，它可以是绝对路径，或相对于执行导入的清单文件的路径。

* remote （必需） - 包含要导入的清单的代码仓库的远程URL

* manifest （必需） - 要导入的清单文件相对于代码仓库根目录的路径。

* name （可选） - 与清单仓库相对应的项目名称。 如果你的清单包含一个具有与清单相同的远程地址的`<project>`标签，那么`<import>`标签的“name”属性需与`<project>`上的“name”属性匹配。 否则，jiri将在每次更新时clone清单仓库。

`<project>`标签描述了需要同步的项目以及根据以下属性，它们应该同步到的状态：

* name （必需） - 项目名。

* path （必需） - 相对于jiri root，项目在本地文件系统上的路径。

* remote （必需） - 项目代码仓库的远程url地址。

* protocol （可选） - clone和同步代码仓库时使用的协议，目前“git”是默认和唯一支持的协议。

* remotebranch （可选） - 项目将要被同步到的远程分支名，默认为“master”分支。 如果指定了“revision”，则“remotebranch”属性将被忽略。

* revision （可选） - 项目将要被同步到的特定版本号（通常是git中的SHA值）。如果指定了“revision”，那么“remotebranch”属性将被忽略。

* gerrithost （可选） - 该项目的Gerrit托管服务URL。在指定URL后，运行“jiri cl upload”将会上传一个CL到该Gerrit服务上。

* githooks （可选） - 包含git hooks的目录的路径（相对于[root]），它将在每次更新时安装到项目的.git/hooks目录中。

`<hook>`标签描述了每个“jiri update”之后需执行的hook，它通过以下的属性进行配置：

* name （必需） - 标识hook的名称

* project （必需） - hook所处的项目名称

* action （必需） - hook在项目内部所执行操作，通常以脚本的形式存在。

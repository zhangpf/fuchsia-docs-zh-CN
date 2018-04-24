<!---
# Jiri Manifest
--->
# Jiri清单
---

[*英文原文快照*](https://github.com/fuchsia-mirror/jiri/blob/6ddcc1e3e9d9c4aba2eb0446b5b1345924c823af/manifest.md)

---

<!---
Jiri manifest files describe the set of projects that get synced when running "jiri update".
--->
Jiri清单文件描述了运行“jiri update”时发生同步的项目集合。

<!---
The first manifest file that jiri reads is in [root]/.jiri\_manifest.  This manifest **must** exist for the jiri tool to work.
--->
jiri读取的第一个清单文件是[root]/.jiri\_manifest，为使jiri工具能够正常工作，该清单**必须**存在。

<!---
Usually the manifest in [root]/.jiri\_manifest will import other manifests from remote repositories via &lt;import> tags, but it can contain its own list of projects as well.
--->
[root]/.jiri\_manifest清单通常会用<import>标签的方式从远程仓库中导入其他的清单文件，但它也可以包含自己的项目列表。

<!---
Manifests have the following XML schema:
--->
清单具有以下XML模式：
<!---
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
--->
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
<!---
The &lt;import> and &lt;localimport> tags can be used to share common projects across multiple manifests.
--->
`<import>`和`<localimport>`标签可用于跨清单共享公共的项目。

<!---
A &lt;localimport> tag should be used when the manifest being imported and the importing manifest are both in the same repository, or when neither one is in a repository.  The "file" attribute is the path to the
manifest file being imported.  It can be absolute, or relative to the importing manifest file.
--->
当被导入的清单和执行导入清单都在同一个代码库中，或者两者都不在代码库中时，应该使用`<localimport>`标签。 “file”属性是被导入的清单文件的路径，它可以是绝对路径，或相对于执行导入的清单文件的路径。

<!---
If the manifest being imported and the importing manifest are in different repositories then an &lt;import> tag must be used, with the following attributes:
--->
如果某个清单被依赖导入，并且它位于不同的仓库中，那么必须使用`<import>`标签，并包含如下的属性：
<!---
* remote (required) - The remote url of the repository containing the manifest to be imported

* manifest (required) - The path of the manifest file to be imported, relative to the repository root.

* name (optional) - The name of the project corresponding to the manifest repository.  If your manifest contains a &lt;project> with the same remote as the manifest remote, then the "name" attribute of on the
&lt;import> tag should match the "name" attribute on the &lt;project>.  Otherwise, jiri will clone the manifest repository on every update.
--->
* remote （必需） - 包含要导入的清单的代码仓库的远程URL

* manifest （必需） - 要导入的清单文件相对于代码仓库根目录的路径。

* name （可选） - 与清单仓库相对应的项目名称。 如果你的清单包含一个具有与清单相同的远程地址的`<project>`标签，那么`<import>`标签的“name”属性需与`<project>`上的“name”属性匹配。 否则，jiri将在每次更新时clone清单仓库。

<!---
The &lt;project> tags describe the projects to sync, and what state they should sync to, accoring to the following attributes:
--->
`<project>`标签描述了需要同步的项目以及根据以下属性，它们应该同步到的状态：

<!---

* name (required) - The name of the project.

* path (required) - The location where the project will be located, relative to the jiri root.

* remote (required) - The remote url of the project repository.

* protocol (optional) - The protocol to use when cloning and syncing the repo. Currently "git" is the default and only supported protocol.

* remotebranch (optional) - The remote branch that the project will sync to. Defaults to "master".  The "remotebranch" attribute is ignored if "revision" is specified.

* revision (optional) - The specific revision (usually a git SHA) that the project will sync to.  If "revision" is  specified then the "remotebranch" attribute is ignored.

* gerrithost (optional) - The url of the Gerrit host for the project.  If specified, then running "jiri cl upload" will upload a CL to this Gerrit host.

* githooks (optional) - The path (relative to [root]) of a directory containing git hooks that will be installed in the projects .git/hooks directory during each update.
--->
* name （必需） - 项目名。

* path （必需） - 相对于jiri root，项目在本地文件系统上的路径。

* remote （必需） - 项目代码仓库的远程url地址。

* protocol （可选） - clone和同步代码仓库时使用的协议，目前“git”是默认和唯一支持的协议。

* remotebranch （可选） - 项目将要被同步到的远程分支名，默认为“master”分支。 如果指定了“revision”，则“remotebranch”属性将被忽略。

* revision （可选） - 项目将要被同步到的特定版本号（通常是git中的SHA值）。如果指定了“revision”，那么“remotebranch”属性将被忽略。

* gerrithost （可选） - 该项目的Gerrit托管服务URL。在指定URL后，运行“jiri cl upload”将会上传一个CL到该Gerrit服务上。

* githooks （可选） - 包含git hooks的目录的路径（相对于[root]），它将在每次更新时安装到项目的.git/hooks目录中。

<!---
The &lt;hook> tag describes the hooks that must be executed after every 'jiri update' They are configured via the following attributes:

* name (required) - The name of the of the hook to identify it

* project (required) - The name of the project where the hook is present

* action (required) - Action to be performed inside the project. It is mostly identified by a script
--->
`<hook>`标签描述了每个“jiri update”之后需执行的hook，它通过以下的属性进行配置：

* name （必需） - 标识hook的名称

* project （必需） - hook所处的项目名称

* action （必需） - hook在项目内部所执行操作，通常以脚本的形式存在。

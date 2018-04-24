# Jiri
---

[*英文原文快照*](https://github.com/fuchsia-mirror/jiri/blob/6ddcc1e3e9d9c4aba2eb0446b5b1345924c823af/behaviour.md)

---
[目录]

## 反馈
（*译者注：该节仅适用于googler，翻译时已省略*）

<!---
## Intended Behavior
--->
## 预期的行为

<!---
### update {#intended-jiri-update}
--->
### 更新

<!---
* Gets latest manifest repository first, then applied below update rules to all local projects.
* Always fetches origin in all the repos except when configured using [`jiri project-config`](#intended-project-config).
* Point the tree name JIRI_HEAD to the manifest selected revision.
* Checkout new repositories at JIRI_HEAD (detached).
* Fast-forward existing repositories to JIRI_HEAD unless further conditions apply (see below).
* If local repo is on a tracked branch, it will fast forward merge to upstream changes. If merge fails, user would be shown a error.
* If project is on un-tracked branch it would be left alone and jiri will show warning.
* It will leave all other local branches as it is.
* If a project is deleted from manifest it  won't be deleted unless command is run with `-gc` flag.
* If a project contains uncommitted changes, jiri will leave it alone and will not fast-forward or merge/rebase the branches.
* Sometimes projects are pinned to particular revision in manifest, in that case if local project is on a local branch, jiri will update them according to above rules and will not throw warnings about those projects.
    * Please note that this can leave projects on revisions other than `JIRI_HEAD` which can cause build failures. In that case user can run [`jiri status`](/howdoi.md#use-jiri-status) which will output all the projects which have changes and/or are not on `JIRI_HEAD`. User can manually checkout `JIRI_HEAD` by running `git checkout JIRI_HEAD` from inside the project.
* If user doesn't want jiri to update a project, he/she can use `jiri project-config`.
* Always updates your jiri tool to latest.
--->
* 获取最新的清单仓库，然后将更新规则应用于所有本地项目。
* 除非使用[`jiri project-config`]进行配置，否则始终在所有项目中获取origin远程仓库的变更。
* 将JIRI_HEAD指针指向清单选定的版本号。
* 在JIRI_HEAD（detached）位置出checkout新的仓库。
* 将现有的仓库快进到JIRI_HEAD状态，除非有更多条件适用（见下文）。
* 如果本地仓库处于被跟踪的分支上，它将使用upstream的变更进行快进合并。如果合并失败，将显示错误给用户。
* 如果项目在未追踪的分支上，将保持不变，但jiri会显示警告信息。
* 所有其他的本地分支将保持不变。
* 如果一个项目从清单中被删除，除非使用`-gc`选项运行命令，它将不会被删除。
* 如果一个项目包含未提交的变更，jiri将其不变，不会快进或merge/rebase该分支。
* 有时项目在清单上被固定为特定的版本，在这种情况下，如果该项目在本地分支上，jiri会根据上述规则更新它们，而不会对这些项目显示警告。
  * 请注意，这可能会导致项目处于除JIRI_HEAD之外的其他版本状态上，进而引发构建失败。在这种情况下，用户可以运行[`jiri status`](/jiri/howdoi.md#检查我的所有项目是否处于jiri_head状态)，它将输出所有发生变更或不处于`JIRI_HEAD`版本的项目。用户可以在项目中运行`git checkout JIRI_HEAD`从而手动checkout出`JIRI_HEAD版本`。
* 如果用户不再想让jiri更新项目，他/她可以使用`jiri project-config`命令。
* 始终更新jiri工具到最新版本。

<!---
#### checkout snapshot {#intended-checkout-snapshot}
Snaphot file captures current state of all the projects. It can be created using command `jiri snapshot`.
--->
#### checkout快照

快照文件用于捕获所有项目的当前状态，它可以通过命令`jiri snapshot`来创建。

<!---
* Snapshot file or a url can be passed to `update` command to checkout snapshot.
* If project has changes, it would **not** be checked-out to snapshot version.
	* else it would be checked out to *DETACHED_HEAD* and snapshot version.
* If `project-config` specifies `ignore` or `noUpdate`, it would be ignored.
* Local branches would **not** be rebased.
--->
* 可以给`update`命令传递快照文件或URL来checkout快照。
* 如果项目发生变化，则**不会**checkout到快照的版本。
    * 否则将被checkout置*DETACHED_HEAD*和快照的版本。
* 如果`project-config`指定了'ignore'或`noUpdate`参数，那么该项目将被忽略。
* 本地分支**不会**发生rebase操作。

<!---
### project-config {#intended-project-config}
--->
### 项目配置
<!---
* If `ignore` is true, jiri will completely ignore this project, ie **no** *fetch*, *update*, *move*, *clean*, *delete* or *rebase*.
* If `noUpdate` is true, jiri will  **not** *fetch*, *update*, *clean* or *rebase* the project.
* For both `ignore` and `noUpdate`, `JIRI_HEAD` is **not** updated for the project.
* If `noRebase` is true, local branches in project **won't be** *updated* or *rebased*.
* This only works with `update` and `project -clean` commands.
--->
* 如果`ignore`设置为true，jiri会完全忽略该项目，即**不再进行fetch、update、move、clean、delete或rebase操作**。
* 如果`noUpdate`设置为true，那么jiri将**不再**对该项目*进行fetch、update、clean 或rebase操作*。
* 对于`ignore`和`noUpdate`中的项目，`JIRI_HEAD`不会被更新。
* 如果`noRebase`设置为true，则项目中的本地分支将**不会**被*update*或*rebase*。
* 以上这仅适用于`update`和`project -clean`命令。

<!---
### project -clean {#intended-project-clean}
--->
### 清理项目

<!---
* Puts projects on `JIRI_HEAD`.
* Removes un-tracked files.
* if `-clean-all` flag is used, force deletes all the local branches, even **master**.
--->
* 将项目置于`JIRI_HEAD`位置。
* 删除未跟踪的文件。
* 如果使用`-clean-all`参数，强制删除所有本地分支，包括**master**分支。

<!---
### upload {#intended-project-upload}
--->
### 上传

<!---
* Sets topic (default *User-Branch*) for each upload unless `set-topic=false` is used.
* Doesn't rebase the changes before uploading unless `-rebase` is passed.
* Uploads multipart change only when `-multipart` is passed.
--->
* 除非使用`set-topic = false`，否则为每次上传设置主题（默认为*User-Branch*）。
* 除非传递“-rebase”参数，否则在上传之前不会rebase变更。
* 仅在传递“-multipart”参数时，才上传multipart变更。

<!---
### patch {#intended-patch}
--->
### 代码补丁

<!---
* Can patch multipart and single changes.
* If topic is provided patch will try to download whole topic and patch all the affected projects, and will try to create branch derived from topic name.
* If topic is not provided default branch would be *change/{id}/{patch-set}*.
* It will **not** rebase downloaded patch-set unless `-rebase` flag is provided.
--->
* 可以给multipart和单个变更进行补丁。
* 如果提供主题，补丁将尝试下载整个主题并修补所有受影响的项目，并尝试创建从主题名称衍生的分支。
* 如果未提供主题，则默认分支将是*change/{id}/{patch-set}*。
* 除非传递“-rebase”参数，否则**不会**rebase下载的补丁集。

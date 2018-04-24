# JIRI
---

[*英文原文快照*](https://github.com/fuchsia-mirror/jiri/blob/6ddcc1e3e9d9c4aba2eb0446b5b1345924c823af/howdoi.md)

---

[目录]

<!---
## How Do I
--->
## 我该如何做

<!---
### rebase current tracked branch instead of fast-forwarding it

`jiri update -rebase-tracked`
--->
### rebase当前跟踪的分支（而不是快进合并）

`jiri update -rebase-tracked`

<!---
### rebase all my branches

Run  `jiri update -rebase-all`. This will not rebase un-tracked branches.
--->
### rebase所有的分支

请运行`jiri update -rebase-all`。但它不会rebase未跟踪的分支。

<!---
### rebase my untracked branches

Run `jiri update -rebase-untracked` to rebase your current un-tracked branch. To rebase all un-tracked branches use `jiri update -rebase-all -rebase-untracked`.
--->
### rebase我的未跟踪的分支

为了rebase当前未跟踪的分支，请运行`jiri update -rebase-untracked`。`jiri update -rebase-all -rebase-untracked`可rebase所有未跟踪的分支。

<!---
### test my local manifest changes

`jiri update -local-manifest`
--->
### 测试本地清单的变更

`jiri update -local-manifest`

<!---
### stop jiri from updating my project

Use `jiri project-config`. [See this](/behaviour.md#intended-project-config) for it's intended behavior.
Current config can be displayed using command `jiri project-config`.
To change a config use
```
jiri project-config [-flag=true|false]
```
where flags are `-ignore`, `no-rebase`, `no-update`
--->
### 停止jiri更新我的项目

使用`jiri project-config`。对于预期的行为请[查看](/jiri/behaviour.md#项目配置)

使用`jiri project-config`命令可显示当前配置信息。

改变配置请使用
```
jiri project-config [-flag=true|false]
```
这里的-flag可以是`-ignore`、`no-rebase`或`no-update`。

<!---
### check if all my projects are on `JIRI_HEAD` {#use-jiri-status}

Run `jiri status ` for that. This command returns all projects which are not on `JIRI_HEAD`, or have un-merged commits, or have un-committed changes.

To just get projects which are **not** on **JIRI_HEAD** run
```
jiri status -changes=false -commits=false
```
--->
### 检查我的所有项目是否处于``JIRI_HEAD`状态

为此请运行`jiri status`，该命令返回所有不在`JIRI_HEAD`状态上、有尚未merge的提交或有未commit的变更的项目。

为了仅返回**未**处于**JIRI_HEAD**状态的项目，请运行：
```
jiri status -changes=false -commits=false
```

<!---
### run a command inside all my projects

`jiri runp [command]`
--->
### 在所有项目中并行运行命令

`jiri runp [command]`

<!---

### grep across projects

Run `jiri grep [text]`. Run `jiri help grep` to see supported flags.
--->
### 跨项目grep

请运行`jiri grep [text]`。运行`jiri help grep`可以查看支持的选项。

<!---
### Run hooks without updating sources
`jiri run-hooks`
--->
### 仅运行hook而不更新代码
`jiri run-hooks`

<!---
### Set hook timeout
`jiri update -hook-timeout=<minutes>` or `jiri run-hooks -hook-timeout=<minutes>`
--->
### 设置hook运行的超时时间

`jiri update -hook-timeout=<minutes>` or `jiri run-hooks -hook-timeout=<minutes>`

<!---

### delete branch across projects

Run `jiri branch -d [branch_name]`, this will run `git branch -d [branch_name]` in all the projects. `-D` can also be used to replicate functionality of `git branch -D`.
--->
### 跨项目删除分支

运行`jiri branch -d [branch_name]`，它会在所有的项目中运行`git branch -d [branch_name]`。`-D`选项同样可以复现`git branch -D`的功能。

<!---
### delete merged branches
`jiri branch -delete-merged`
--->
### 删除已合并的分支
`jiri branch -delete-merged`

<!---
### delete merged branches with different commits
Run `jiri branch -delete-merged-cl`. This will check gerrit and delete all those branches whose commits have been submitted, by matching gerrit change list ID. **Use this with caution**.
--->
### 使用新的提交来删除合并后的分支
请运行`jiri branch -delete-merged-cl`，它将通过匹配gerrit变更列表ID的方式，来检查gerrit和删除所有已提交commit的分支。**请谨慎使用**

<!---
### get projects and branches other than master

`jiri branch`
--->
### 获取项目除master之外的分支

`jiri branch`

<!---
### find difference between two snapshots
Run `jiri diff <old_snapshot> <new_snapshot>`. *old_snapshot* and *new_snapshot* can be file paths or urls.
--->
### 查找两份快照的差异
请运行`jiri diff <old_snapshot> <new_snapshot>`。*old_snapshot*和*new_snapshot*可以是文件路径或URL地址。 

<!---
### download whole gerrit topic

`jiri patch -topic <topic>`
--->
### 下载gerrit的整个主题

`jiri patch -topic <topic>`

<!---
### update jiri without updating projects

`jiri selfupdate`
--->
### 不更新项目的前提下更新jiri

`jiri selfupdate`

<!--
### use upload to push CL

[See This](/README.md#Gerrit-CL-workflow)
--->
### 上传push CL

请[查看](/jiri/README.md#gerrit-cl工作流)

<!---
### get JIRI_HEAD revision of a project

`git rev-parse JIRI_HEAD` from inside the project.
--->
### 获得项目的JIRI_HEAD的版本号

在项目目录内运行`git rev-parse JIRI_HEAD`

<!---
### get current revision of a project

`jiri project [project-name]`
--->
### 获取项目的当前版本

`jiri project [project-name]`

<!---
### clean project(s)

Run `jiri project [-clean|clean-all] [project-name]`. See it's [intended behaviour](/behaviour.md#intended-project-clean).
--->
### 清理项目

运行`jiri project [-clean|clean-all] [project-name]`。请查看[预期的行为](/jiri/behaviour.md#清理项目)。

<!---
### get help

Run `jiri help` to see all the commands and `jiri help [command]` to get help for that command.
--->
### 获得帮助

运行`jiri help`来查看所有的命令，运行`jiri help [command]`来获取某个项目的帮助文档。

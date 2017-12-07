# JIRI

[目录]

## 我该如何做

### rebase当前跟踪的分支（而不是快进合并）

`jiri update -rebase-tracked`

### rebase所有的分支

请运行`jiri update -rebase-all`。但它不会rebase未跟踪的分支。

### rebase我的未跟踪的分支

为了rebase当前未跟踪的分支，请运行`jiri update -rebase-untracked`。`jiri update -rebase-all -rebase-untracked`可rebase所有未跟踪的分支。

### 测试本地清单的变更

`jiri update -local-manifest`

### 停止jiri更新我的项目

使用`jiri project-config`。对于预期的行为请[查看](/behaviour.md#intended-project-config)

使用`jiri project-config`命令可显示当前配置信息。

改变配置请使用
```
jiri project-config [-flag=true|false]
```
这里的-flag可以是`-ignore`、`no-rebase`或`no-update`。

### 检查我的所有项目是否处于``JIRI_HEAD`状态{#use-jiri-status}

为此请运行`jiri status`，该命令返回所有不在`JIRI_HEAD`状态上、有尚未merge的提交或有未commit的变更的项目。

为了仅返回**未**处于**JIRI_HEAD**状态的项目，请运行：
```
jiri status -changes=false -commits=false
```
### 在所有项目中并行运行命令

`jiri runp [command]`

### 跨项目grep

请运行`jiri grep [text]`。运行`jiri help grep`可以查看支持的选项。

### 仅运行hook而不更新代码
`jiri run-hooks`

### 设置hook运行的超时时间

`jiri update -hook-timeout=<minutes>` or `jiri run-hooks -hook-timeout=<minutes>`

### 跨项目删除分支

运行`jiri branch -d [branch_name]`，它会在所有的项目中运行`git branch -d [branch_name]`。`-D`选项同样可以复现`git branch -D`的功能。

### 删除已合并的分支
`jiri branch -delete-merged`

### 使用新的提交来删除合并后的分支
请运行`jiri branch -delete-merged-cl`，它将通过匹配gerrit变更列表ID的方式，来检查gerrit和删除所有已提交commit的分支。**请谨慎使用**

### 获取项目除master之外的分支

`jiri branch`

### 查找两份快照的差异
请运行`jiri diff <old_snapshot> <new_snapshot>`。*old_snapshot*和*new_snapshot*可以是文件路径或URL地址。 

### 下载gerrit的整个主题

`jiri patch -topic <topic>`

### 不更新项目的前提下更新jiri

`jiri selfupdate`

### 上传push CL

请[查看](/README.md#Gerrit&nbsp;CL工作流)

### 获得项目的JIRI_HEAD的版本号

在项目目录内运行`git rev-parse JIRI_HEAD`

### 获取项目的当前版本

`jiri project [project-name]`

### 清理项目

运行`jiri project [-clean|clean-all] [project-name]`。请查看[预期的行为](/behaviour.md#intended-project-clean)。

### 获得帮助

运行`jiri help`来查看所有的命令，运行`jiri help [command]`来获取某个项目的帮助文档。

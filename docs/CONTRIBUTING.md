<!---
Contributing Changes
--->
# 贡献代码变更
---

[*英文原文快照*](https://github.com/fuchsia-mirror/docs/blob/d6693145a6db79ef6db798683c743e640f7d4f96/CONTRIBUTING.md)

---

<!---
Fuchsia manages commits through Gerrit at
https://fuchsia-review.googlesource.com. Not all projects accept patches;
please see the CONTRIBUTING.md document in individual projects for
details.
--->
Fuchsia通过[Gerrit](https://fuchsia-review.googlesource.com)来管理代码提交。需要说明的是并非所有的子项目都接收补丁（patch），具体详情请查看每个子项目的CONTRIBUTING.md文档。

<!---
## Submitting changes
--->
## 提交代码变更

<!---
To submit a patch to Fuchsia, you may first need to generate a cookie to
authenticate you to Gerrit. To generate a cookie, log into Gerrit and click
the "Generate Password" link at the top of https://fuchsia.googlesource.com.
Then, copy the generated text and execute it in a terminal.
--->
为了向Fuchsia提交patch，需要首先生成授权你到Gerrit的cookie项。为此，请首先登录到Gerrit，然后点击位于[https://fuchsia.googlesource.com](https://fuchsia.googlesource.com)顶部的“Generate Password”的链接。然后拷贝生成的命令到终端并执行。

<!---
Once authenticated, follow these steps to submit a patch to a repo in Fuchsia:
--->
一旦授权成功，便可通过以下的步骤向Fuchsia的某个子项目提交patch。

<!---
```
# create a new branch
git checkout -b branch_name

# write some awesome stuff, commit to branch_name
# edit some_file ...
git add some_file
# if specified in the repo, follow the commit message format
git commit ...

# upload the patch to Gerrit
# `jiri help upload` lists flags for various features, e.g. adding reviewers
jiri upload # Adds default topic - ${USER}-branch_name
# or
jiri upload -topic="custom_topic"
# or
git push origin HEAD:refs/for/master

# at any time, if you'd like to make changes to your patch, use --amend
git commit --amend

# once the change is landed, clean up the branch
git branch -d branch_name
```
--->
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
<!---
See the Gerrit documentation for more detail:
[https://gerrit-documentation.storage.googleapis.com/Documentation/2.12.3/intro-user.html#upload-change](https://gerrit-documentation.storage.googleapis.com/Documentation/2.12.3/intro-user.html#upload-change)
--->
更多细节请查看Gerrit的文档：
[https://gerrit-documentation.storage.googleapis.com/Documentation/2.12.3/intro-user.html#upload-change](https://gerrit-documentation.storage.googleapis.com/Documentation/2.12.3/intro-user.html#upload-change)

<!---
### Change description tags

If submitting a change to Zircon, Garnet, Peridot or Topaz, include [tags] in
the commit subject flagging which module, library, app, etc, is affected by the
change. The style here is somewhat informal. Look at these example changes to
get a feel for how these are used.
--->
### 变更的描述性tag

当向Zircon、Garnet、Peridot或Topaz提交变更时候，请在commit的主题中标识被影响的module、library和app相关的[tag]。这种风格稍微有点非正式，因此请查看如下的变更示例来了解它们是如何使用的。

<!---
* https://fuchsia-review.googlesource.com/c/zircon/+/112976
* https://fuchsia-review.googlesource.com/c/garnet/+/110795
* https://fuchsia-review.googlesource.com/c/peridot/+/113955
* https://fuchsia-review.googlesource.com/c/topaz/+/114013
--->
* https://fuchsia-review.googlesource.com/c/zircon/+/112976
* https://fuchsia-review.googlesource.com/c/garnet/+/110795
* https://fuchsia-review.googlesource.com/c/peridot/+/113955
* https://fuchsia-review.googlesource.com/c/topaz/+/114013

<!---
Gerrit will flag your change with
`Needs Label: Commit-Message-has-tags` if these are missing.
--->
如果变更未指明这些[tag]，Gerrit将会用`Needs Label: Commit-Message-has-tags`来标识该变更。

<!---
Example:
```
# Ready to submit
[parent][component] Update component in Topaz.

# Needs Label: Commit-Message-has-tags
Update component in Topaz.
```
--->
例如：

```
# 提交准备
[parent][component] Update component in Topaz.

# Needs Label: Commit-Message-has-tags
Update component in Topaz.
```

<!---
## [Non-Googlers only] Sign the Google CLA

In order to land your change, you need to sign the [Google CLA](https://cla.developers.google.com/).
--->
## [仅针对非Googlers] 签署Google贡献者许可证协议（CLA）

在上传代码变更之前，请首先签署Google贡献者许可证协议（[Google CLA](https://cla.developers.google.com/))。

<!---
## Cross-repo changes

Changes in two or more separate repos will be automatically tracked for you by
Gerrit if you use the same topic.
--->
## 跨仓库变更

对于两个或多个不同的子项目仓库的变更，如果你使用了相同的话题，Gerrit会为你自动进行跟踪。

<!---
### Using jiri upload
Create branch with same name on all repos and upload the changes
--->
### 使用jiri进行上传

在所有的子项目上创建相同名字的分支，然后提交变更
<!---
```
# make and commit the first change
cd fuchsia/bin/fortune
git checkout -b add_feature_foo
* edit foo_related_files ... *
git add foo_related_files ...
git commit ...

# make and commit the second change in another repository
cd fuchsia/build
git checkout -b add_feature_foo
* edit more_foo_related_files ... *
git add more_foo_related_files ...
git commit ...

# Upload all changes with the same branch name across repos
jiri upload -multipart # Adds default topic - ${USER}-branch_name
# or
jiri upload -multipart -topic="custom_topic"

# after the changes are reviewed, approved and submitted, clean up the local branch
cd fuchsia/bin/fortune
git branch -d add_feature_foo

cd fuchsia/build
git branch -d add_feature_foo
```
--->
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
<!---
### Using Gerrit commands
--->
### 使用Gerrit的命令

<!---
```
# make and commit the first change, upload it with topic 'add_feature_foo'
cd fuchsia/bin/fortune
git checkout -b add_feature_foo
* edit foo_related_files ... *
git add foo_related_files ...
git commit ...
git push origin HEAD:refs/for/master%topic=add_feature_foo

# make and commit the second change in another repository
cd fuchsia/build
git checkout -b add_feature_foo
* edit more_foo_related_files ... *
git add more_foo_related_files ...
git commit ...
git push origin HEAD:refs/for/master%topic=add_feature_foo

# after the changes are reviewed, approved and submitted, clean up the local branch
cd fuchsia/bin/fortune
git branch -d add_feature_foo

cd fuchsia/build
git branch -d add_feature_foo
```
--->
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
<!---
Multipart changes are tracked in Gerrit via topics, will be tested together,
and can be landed in Gerrit at the same time with `Submit Whole Topic`. Topics
can be edited via the web UI.
--->
变更的多个部分在Gerrit中通过相同话题进行追踪，并且也会一起进行测试，也可以通过`Submit Whole Topic`向Gerrit提交它们。另外，还可以通过网页编辑话题。

<!---
## Changes that span layers
--->
## 跨多个层的变更

<!---
See [Changes that span layers](development/workflows/multilayer_changes.md) on
how to work across [layers](development/source_code/layers.md).
--->

请查看[该文档](development/workflows/multilayer_changes.md)来了解如何处理跨[layer](development/source_code/layers.md)。

<!---
## Resolving merge conflicts
--->
## 消解合并冲突

<!---
```
# rebase from origin/master, revealing the merge conflict
git rebase origin/master

# resolve the conflicts and complete the rebase
* edit files_with_conflicts ... *
git add files_with_resolved_conflicts ...
git rebase --continue
jiri upload

# continue as usual
git commit --amend
jiri upload
```
--->
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

<!---
## Github integration

While Fuchsia's code is hosted at https://fuchsia.googlesource.com, it is also
mirrored to https://github.com/fuchsia-mirror. To ensure Fuchsia contributions
are associated with your Github account:
--->
## Github集成

除了部署于[https://fuchsia.googlesource.com](https://fuchsia.googlesource.com)的Fuchsia代码之外，同时在Gihub也有Fuchsia的镜像[https://github.com/fuchsia-mirror](https://github.com/fuchsia-mirror)。为了保证Fuchsia的贡献同时也关联到你的Github账号：

<!---
1. [Set your email in Git](https://help.github.com/articles/setting-your-email-in-git/).
2. [Adding your email address to your GitHub account](https://help.github.com/articles/adding-an-email-address-to-your-github-account/).
3. Star the project for your contributions to show up in your profile's
Contribution Activity.
--->
1. [在git中设置好邮箱](https://help.github.com/articles/setting-your-email-in-git/)；
2. [将你的邮箱地址加入到Github账号中](https://help.github.com/articles/adding-an-email-address-to-your-github-account/)；
3. 为了使你的贡献出现在主页的贡献活动（Contribution Activity）中，请标星该子项目。

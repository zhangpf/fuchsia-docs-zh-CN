<!---
# Jiri filesystem
--->
# Jiri文件系统
---

[*英文原文快照*](https://github.com/fuchsia-mirror/jiri/blob/6ddcc1e3e9d9c4aba2eb0446b5b1345924c823af/filesystem.md)

---

<!---
All data managed by the jiri tool is located in the file system under a root directory, colloquially called the jiri root directory.  The file system layout looks like this:
--->
所有jiri工具管理的数据都位于文件系统的一个[root]目录下，通俗地称该目录为jiri根目录。文件系统布局如下所示：
<!---
```
 [root]                                   # root directory (name picked by user)
 [root]/.jiri_root                        # root metadata directory
 [root]/.jiri_root/bin                    # contains jiri tool binary
 [root]/.jiri_root/update_history         # contains history of update snapshots
 [root]/.manifest                         # contains jiri manifests
 [root]/[project1]                        # project directory (name picked by user)
 [root]/[project1]/.git/jiri              # project metadata directory
 [root]/[project1]/.git/jiri/metadata.v2  # project metadata file
 [root]/[project1]/.git/jiri/config       # project local config file
 [root]/[project1]/<<files>>              # project files
 [root]/[project2]...
```
--->
```
 [root]                                   # 根目录（由用户选定的名字）
 [root]/.jiri_root                        # 根目录下jiri元数据目录
 [root]/.jiri_root/bin                    # 包含jiri工具的二进制文件
 [root]/.jiri_root/update_history         # 包含jiri更新快照的历史记录
 [root]/.manifest                         # jiri清单
 [root]/[project1]                        # 项目的目录（由用户选定的名字）
 [root]/[project1]/.git/jiri              # 项目的git元数据目录
 [root]/[project1]/.git/jiri/metadata.v2  # 项目的git元数据文件
 [root]/[project1]/.git/jiri/config       # 项目的git本地配置文件
 [root]/[project1]/<<files>>              # 项目其他文件
 [root]/[project2]...
```

<!---
The [root] and [projectN] directory names are picked by the user.  The &lt;&lt;cls>> are named via jiri cl new, and the &lt;&lt;files>> are named as the user adds files and directories to their project.  All other names
above have special meaning to the jiri tool, and cannot be changed; you must ensure your path names don't collide with these special names.
--->
[root]和[projectN]目录名由用户选定，`<<cls>>`是jiri cl new命名的文件，`<<files>>`是用户添加到项目的文件和目录。除此之外，以上所有其他名称对jiri工具有着特殊的含义，且不能改变; 所以你必须确保你的路径名称不与这些特殊名称相冲突。


<!---
To find the [root] directory, the jiri binary looks for the .jiri\_root directory, starting in the current working directory and walking up the directory chain.  The search is terminated successfully when the
.jiri\_root directory is found; it fails after it reaches the root of the file system. Thus jiri must be invoked from the [root] directory or one of its subdirectories.  To invoke jiri from a different
directory, you can set the -root flag to point to your [root] directory.
--->
为了查找[root]目录，jiri二进制文件将从当前目录开始，沿着目录链向上查找.jiri\_root目录。当找到.jiri\_root目录时，搜索成功并终止；否则在到达文件系统的根目录后搜索失败。因此必须从[root]目录或其子目录调用jiri，如果要从其他目录调用jiri，可以设置-root选项，并指向[root]目录。

<!---
Keep in mind that when "jiri update" is run, the jiri tool itself is automatically updated along with all projects.  Note that if you have multiple [root] directories on your file system, you must remember to
run the jiri binary corresponding to your [root] directory.  Things may fail if you mix things up, since the jiri binary is updated with each call to "jiri update", and you may encounter version mismatches
between the jiri binary and the various metadata files or other logic.
--->
请记住，当“jiri update”运行时，jiri工具本身会自动地和所有项目一起被更新。请注意，如果文件系统上有多个[root]目录，那么必须记得运行与[root]目录对应的某个jiri二进制文件。如果你将它们混在一起可能会导致失败，因为在每次调用“jiri update”时jiri都会更新二进制文件，所以你可能会遭遇到各个jiri二进制文件之间各种元数据文件或其他逻辑的版本不匹配问题。

<!---
The jiri binary is located at [root]/.jiri\_root/bin/jiri
--->
jiri二进制文件位于[root]/.jiri\_root/bin/jiri

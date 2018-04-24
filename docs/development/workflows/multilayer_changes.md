<!---
# Working on multiple layers
--->
# 跨多个层的变更
---

[*英文原文快照*](https://raw.githubusercontent.com/fuchsia-mirror/docs/a774512b9d926ee438a77ddc6a5f362b71e0cc4b/development/workflows/multilayer_changes.md)

---
<!---
## Switching between layers
--->
## 在层之间切换

<!---
When you bootstrapped your development environment (see
[getting source][getting-source]), you selected a layer. Your development
environment views that layer at the latest revision and views the lower layers
at specific revisions in the past.
--->

在初始化开发环境（详情请查看[获取代码](https://github.com/fuchsia-mirror/docs/blob/a774512b9d926ee438a77ddc6a5f362b71e0cc4b/getting_source.md)）步骤中，你已选择了一个层（layer）进行开发。该层的代码处于最新版本，而它下面的层则处于过去的某个历史版本中（*译者注：类似于git中的submodule，子模块并不会直接更新到最新版本*）。

<!---
If you want to switch to working on a different layer, either to get the source
code for higher layers in your source tree or to see lower layers at more recent
revisions, you have two choices:
--->
如果你想切换到其他层，要么在代码树中获取更高层的源代码，要么当前查看的是更低层的的某个历史版本，对此你有两个选择：

<!---
1. You can bootstrap a new development environment for that layer using
   [the same instructions you used originally][getting-source].
2. You can modify your existing development environment using the
   `fx set-layer <layer>` command. This command edits the `jiri` metadata for
   your source tree to refer to the new layer and prints instructions for how to
   actually get the source and build the newly configured layer.
--->
1. 采用和之前一样初始化的方式，[为该层初始化新的开发环境](https://github.com/fuchsia-mirror/docs/blob/a774512b9d926ee438a77ddc6a5f362b71e0cc4b/getting_source.md)。
2. 通过`fx set-layer <layer>`命令切换现有的开发环境。该命令通过改变代码树`jiri`元信息来指向新的层，并显示如何真正获取代码和构建新配置层的说明。

<!---
## Changes that span layers
--->
## 跨层的代码变更

<!---
Fuchsia is divided into a number of [layers][layers]. Each layer views the
previous layers at pinned revisions, which means changes that land in one layer
are not immediately visible to the upper layers.
--->
因为Fuchsia被分成多[层](../source_code/layers.md)，每层看到的它下面的层其实是被固定到某个特定的历史版本，也就是说一个层更新后的代码并不会立即被上方的层看到。

<!---
When making a change that spans layers, you need to think about when the
differnet layers will see the different parts of you change. For example,
suppose you want to change an interface in Zircon and affects clients in Garnet.
When you land your change in Zircon, people building Garnet will not see your
change immediately. Instead, they will start seeing your change once Garnet
updates its revision pin for Zircon.
--->
当创建层的变更时，你需要考虑清楚在何时不同层会看到变更的不同部分，例如，当你想改变Zircon的接口，并且影响到Gernet的上层客户代码时，其他开发者在构建Gernet时并不会立刻看到Zircon的提交。相反，只有在Gernet更新了它指向Zricon的版本时才会看到这些更新。

<!---
### Soft transitions (preferred)
--->
### 软变换（推荐）

<!---
The preferred way to make changes that span multiple layers is to use a
*soft transition*. In a soft transition, you make a change to the lower layer
(e.g., Zircon) in such a way that the interface supports both old and new
clients. For example, if you are replacing a function, you might add the new
version and turn the old function into a wrapper for the new function.
--->
对于跨层变更的一种较好方式是使用*软变换*。在这种方式下，你提交到较低层（例如Zircon）的代码变更会同时支持新旧两种接口使用方式。例如，如果你想替换某个函数，你可以增加该函数的新版本，并用新版本来封装旧版本。

<!---
Use the follow steps to land a soft transition:
--->
采用如下的步骤来进行软变换：

<!---
1. Land the change in the lower layer (e.g., Zircon) that introduces the new
   interface without breaking the old interface used by the upper layer
   (e.g., Garnet).
2. Wait for the autoroll bot to update the revision of the lower layer
   used by the upper layer.
3. Land the change to the upper layer that migrates to the new interface.
4. Land a cleanup change in the lower layer that removes the old interface.
--->
1. 提交较低层层（例如Zircon）的代码变更，在不破坏上层（例如Garnet）使用的旧接口前提下，引入新的接口；
2. 等待自动回滚bot更新上层指向的下层的版本号；
3. 提交更改上层的代码，迁移到新的接口；
4. 提交底层的变更，删掉旧接口。

<!---
### Hard transitions
--->
### 硬变换

<!---
For some changes, creating a soft transition can be difficult or impossible. For
those changes, you can make a *hard transition*. In a hard transition, you make
a breaking change to the lower layer and update the upper layer manually.

Use the follow steps to land a hard transition:
--->
对于某些变更，创建软变换是非常困难或者根本不可行的，对此，你可以使用*硬变换*方式。在这种方式下，你创建破坏性变更来更新底层代码，并手动更新上层代码。

采用如下的步骤来进行硬变换：

<!---
1. Land the change in the lower layer (e.g., Zircon) that breaks the interface
   used by the upper layer (e.g., Garnet). At this point, the autoroll bot will
   start failing to update the upper layer.
3. Land the change to the upper layer that both migrates to the new interface
   and updates the revision of the lower layer used by the upper layer by
   editing the `revision` attribute for the import of the lower layer in the
   `//<layer>/manifest/<layer>` manifest of the upper layer.
--->
1. 上传底层（例如Zircon）破坏上层使用接口的变更。此时，自动回滚bot的更新上层操作将会失败；
2. 上传上层的变更，迁移到新的接口，以及通过编辑上层的`//<layer>/manifest/<layer>` 清单文件中底层的`revision`属性来更新指向底层的版本号。

<!---
Making a hard transition is more stressful than making a soft transition because
your change will be preventing other changes in the lower layer from becoming
available in the upper layers between steps 1 and 2.
--->
进行硬变换比软变换会产生更大的压力，因为在步骤1和步骤2之间将会阻止其他代码变更的发生。
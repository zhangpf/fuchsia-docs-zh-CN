<!-- # Scenic, the Fuchsia graphics engine -->
# Scenic，Fuchsia的图形引擎
---

[*英文原文快照*](https://github.com/fuchsia-mirror/garnet/blob/ae95d58984f6fc99df2f79dbe79c6c5406c0cf04/docs/ui_scenic.md)

---
<!-- 
Scenic is a Garnet service that composes graphical objects from multiple
processes into a shared scene graph.  These objects are rendered within a
unified lighting environment (to a display or other render target); this
means that the objects can cast shadows or reflect light onto each other,
even if the originating processes have no knowledge of each other. -->
Scenic是将来自多个进程的图形对象组合成共享场景图的Garnet服务。这些对象在统一的光照环境下渲染（到显示器或其他渲染目标上）。这意味着即使原始进程彼此相互不了解，对象也可以投射阴影或将光反射到彼此之上。

<!-- Scenic's responsibilities are: -->
Scenic的职责包括：
<!-- - Composition: Scenic provides a retained-mode 3D scene graph that contains
  content that is independently generated and linked together by its
  clients.  Composition makes it possible to seamlessly intermix
  the graphical content of separately implemented UI components. -->

- 合成：Scenic提供保留模式的3D场景图，其中包含由客户独立生成和链接而成的内容。合成使得无缝混合多个单独实现的UI组件的图形内容变得可能。

<!-- - Animation: Scenic re-evaluates any time-varying expressions within models
  prior to rendering each frame, thereby enabling animation of model
  properties without further intervention from clients.  Offloading
  animations to Scenic ensures smooth jank-free transitions. -->

- 动画：在渲染每个帧之前，Scenic会重计算模型中的任何时变表达式，从而实现无需客户端的进一步干预即可启用模型属性的动画。将动画卸载到Scenic可确保平稳无抖动的转换。
  
<!-- - Rendering: Scenic renders its scene graph using Escher, a rendering
  library built on Vulkan, which applies realistic lighting and shadows to
  the entire scene. -->

- 渲染：Scenic使用Escher渲染其场景图。Escher是一个基于Vulkan的渲染库，它将逼真的光照和阴影应用到整个场景中。

<!-- - Scheduling: Scenic schedules scene updates and animations to anticipate
  and match the rendering target's presentation latency and refresh interval. -->

- 调度：Scenic调度场景的更新和动画，以预测和匹配渲染目标的演示延迟和刷新间隔。

<!-- - Diagnostics: Scenic provides a diagnostic interface to help developers
  debug their models and measure performance. -->

- 诊断：Scenic提供诊断界面，以帮助开发人员调试模型和衡量性能。

<!-- # Concepts -->
# 概念

## Scenic

<!-- The `SceneManager` FIDL interface is Scenic's front door.  Each instance
of the interface represents a Scenic instance (we plan to rename the
interface to `Scenic` soon). Each Scenic instance is an isolated rendering
context with its own content, render targets, and scheduling loop. -->
<!-- The following operations are provided: -->

名为`SceneManager`的FIDL接口是Scenic的前端，接口中的每个实例均代表一个Scenic实例（我们计划尽快将接口重命名为`Scenic`）。每个Scenic实例都是一个独立的渲染上下文，具有自己的内容、渲染目标和调度循环。并提供以下操作：
<!-- - Create client `Sessions` to publish graphical content to this instance.
- Create rendering targets to specify where the output of the instance
  should be rendered, such as to a display or to an image (e.g. screen
  shots).
- Bind scenes to rendering targets.
- Obtain another Zircon channel which is bound to the same Scenic
  instance.  (Duplicate) -->
- 创建客户端的`Session`，用于将图形内容发布该实例。
- 创建渲染目标以指定实例被渲染后输出的位置，例如显示器或图像（如屏幕截图）。
- 将场景绑定到渲染目标上。
- 获得另一个绑定到同一个Scenic实例上的Zircon通道（复制）。

<!-- A single Scenic instance can update, animate, and render multiple
`Scenes` (trees of graphical objects) to multiple targets in tandem on the same
scheduling loop.  This means that the timing model for a Scenic instance
is _coherent_: all of its associated content belongs to the same scheduling
domain and can be seamlessly intermixed. -->
单个Scenic实例可以在同一个调度循环中更新，动画和渲染多个`Scene`（图形对象树）到多个串联而成目标上。这意味着Scenic实例的时序模型是*一致的*：其所有相关内容属于同一个调度域内，并可以无缝进行混合。

<!-- Conversely, independent Scenic instances cannot share content and are
therefore not coherent amongst themselves.  Creating separate Scenic
instances can be useful for rendering to targets which have very different
scheduling requirements or for running tests in isolation. -->
相反地，独立的Scenic实例不能共享内容，因此它们之间是不一致的。创建单独的Scenic实例对于渲染具有相当不同调度要求的或需单独运行测试的目标非常有用。

<!-- When a Scenic instance is destroyed, all of its sessions become
inoperable and its rendering ceases. -->
当Scenic实例被销毁时，其所有会话都将无法运行，同时渲染也将停止。

<!-- `Views` typically do not deal with the Scenic instance directly; instead
they receive a Scenic `Session` from the view manager. -->
`View`通常不直接处理Scenic实例。相反，他们从视图管理器处获得Scenic的`Session`实例。

<!-- ## Sessions -->
## 会话 (`Session`)

<!-- The `Session` FIDL interface is the primary API used by clients of Scenic to
contribute graphical content in the form of `Resources`.  Each session has
its own resource table and is unable to directly interact with resources
belonging to other sessions. -->
`Session`的FIDL接口是Scenic客户端使用的主要API，以`Resource`的形式提供图形内容。每个会话都有自己的资源表，并且无法直接与属于其他会话的资源进行交互。

<!-- Each session provides the following operations: -->
每个会话提供如下的操作：

<!-- - Submit operations to add, remove, or modify resources.
- Commit a sequence of operations to be presented atomically.
- Awaiting and signaling fences.
- Schedule subsequent frame updates.
- Form links with other sessions (by mutual agreement). -->
- 提交添加、删除或修改资源的操作。
- 提交一系列以原子方式呈现的操作。
- 等待和发送栅栏信号singal。
- 调度后续帧的更新。
- 与其他会话（通过协商）形成链接。

<!-- When a session is destroyed, all of its resources are released and all of
its links become inoperable. -->
会话被销毁时，其所有资源都将被释放，并且所有链接都无法运行。

<!-- `Views` typically receive separate sessions from the view manager. -->
`View`通常从视图管理器处接收单独的会话。

<!-- ## Resources -->
## 资源 (`Resource`)

<!-- `Resources` represent scene elements such as nodes, shapes, materials, and
animations which belong to particular `Sessions`. -->
`Resource`表示场景中的元素，例如属于特定`Session`的节点、形状、材质和动画等。

<!-- 
The list of Scenic resources is described by the API:
//garnet/public/fidl/fuchsia.ui.gfx/resources.fidl -->
位于`//garnet/public/fidl/fuchsia.ui.gfx/resources.fidl`的API描述了Scenic的资源列表。

<!-- Clients of Scenic generate graphical content to be rendered by queuing and
submitting operations to add, remove, or modify resources within their
session. -->
Scenic的客户端通过排队和提交操作来生成要呈现的图形内容，以添加、删除或修改其会话中的资源。
<!-- 
Each resource is identified within its session by a locally unique id which
is assigned by the owner of the session (by arbitrary means).  Sessions
cannot directly refer to resources which belong to other sessions (even if
they happen to know their id) therefore content embedding between sessions
is performed using `Link` objects as intermediaries. -->
每个资源在其会话中由本地唯一ID标识，该ID由会话所有者（通过任意方式）分配。会话不能直接引用属于其他会话的资源（即使它知道这些资源的ID）。因此会话之间的内容嵌入是通过使用`Link`对象作为中介来执行的。

<!-- To add a resource, perform the following steps: -->
为了添加资源，需要执行以下步骤：

<!-- - Enqueue an operation to add a resource of the desired type and assign it a
  locally unique id within the session.
- Enqueue one or more operations to set that resource's properties given its
  id. -->
- 将操作排队以添加所需类型的资源，并在会话中为其分配唯一本地ID。
- 将一个或多个操作排入队列，在指定其ID的条件下设置该资源的属性。
  
<!-- Certain more complex resources may reference the ids of other resources
within their own definition.  For instance, a `Node` references its `Shape`
thus the `Shape` must be added before the `Node` so that the node may
reference it as part of its definition. -->
某些更复杂的资源可能会在其自定义中引用其他资源的ID。例如，`Node`引用它的`Shape`，因此必须在`Node`之前添加`Shape`，以便`Node`可以将它作为其定义的一部分引用。

<!-- To modify a resource, enqueue one or more operations to set the desired
properties in the same manner used when the resource was added. -->
要修改资源，请将一个或多个操作排入队列，以便以添加资源时使用的相同方式设置所需的属性。

<!-- The remove a resource, enqueue an operation to remove the resource. -->

<!-- Removing a resource causes its id to become available for reuse.  However,
the session maintains a reference count for each resource which is
internally referenced.  The underlying storage will not be released (and
cannot be reused) until all remaining references to the resource have been
cleared *and* until the next frame which does not require the resource has
been presented.  This is especially important for `Memory` resources.
See also [Fences](#fences). -->
为了删除资源，需要将操作排入队列。删除资源会导致其ID重新变成重用状态。但是，会话维护了每个内部引用资源的引用计数，底层存储在所有剩余资源的引用被清除*和*直到下一帧不需要依赖这些资源之前将不会被释放（并且不能被重用）。这对于`Memory`资源尤为重要，请另参见[栅栏](#栅栏)。


<!-- This process of addition, modification, and removal may be repeated
indefinitely to incrementally update resources within a session. -->
添加、修改和删除过程可以无限次地重复，以递增地更新会话内的资源。

<!-- ### Nodes -->
### 节点 (`Node`)

<!-- A `Node` resource represents a graphical object which can be assembled into
a hierarchy called a `node tree` for rendering.

TODO: Discuss this in more detail, especially hierarchical modeling concepts
such as per-node transforms, groups, adding and removing children, etc. -->
`Node`资源表示可以组装成名为节点树(`node tree`)，并用于渲染的层次结构的图形对象。

TODO：更详细地讨论这个问题，尤其是分层建模概念，例如每个节点的转换、分组、添加和删除子节点等。

<!-- ### Scenes -->
### 场景 (`Scene`)

<!-- A `Scene` resource combines a tree of nodes with the scene-wide parameters
needed to render it.  A Scenic instance may contain multiple scenes but
each scene must have its own independent tree of nodes. -->
`Scene`资源将节点树(`node tree`)与渲染它所需的场景范围参数组合在一起。一个Scenic实例可能包含多个场景，但每个场景必须有自己独立的节点树。

<!-- A scene resource has the following properties: -->
场景资源具有以下属性：
<!-- - The scene's root node.
- The scene's global parameters such as its lighting model. -->
- 场景的根node节点。
- 场景的全局参数，例如其照明模型。

<!-- In order to render a scene, a `Camera` must be pointed at it. -->
为了对一个场景进行渲染，必须有`Camera`对象指向它。

<!-- ### Compositors -->
### 合成器

<!-- Compositors are resources that come in two flavors: `DisplayCompositor` and
`ImagePipeCompositor`; their job is to draw the content of a `LayerStack`
into their render target.  For `DisplayCompositor`, the target display may
have multiple hardware overlays; in this case the compositor may choose
associate each of these with a separate layer, rather than flattening the
layers into a single image. -->
合成器是两种形式的资源：`DisplayCompositor`和`ImagePipeCompositor`。它们的工作是将`LayerStack`的内容绘制到渲染目标中。对于`DisplayCompositor`而言，目标显示器可能有多个硬件覆盖：在这种情况下，合成器可以选择将这些中的每一个与单独的层相关联，而不是将这些层平展为单个图像。

<!-- A `LayerStack` resource consists of an ordered list of `Layers`.  Each layer
can contain either an `Image` (perhaps transformed by a matrix), or a
`Camera` that points at a `Scene` to be rendered (as described above). -->
`LayerStack`资源由`Layers`的有序列表组成。每一层都可以包含一个`Image`（可能由矩阵变换而成），或者一个指向要渲染`Scene`的`Camera`资源（如上所述）。

<!-- ### TODO: More Resources -->
### TODO：更多资源

<!-- Add sections to discuss all other kinds of resources: shapes, materials,
links, memory, images, buffers, animations, variables, renderers etc. -->
添加相应的章节以讨论所有其他类型的资源，包括形状、材料、链接、内存、图像、缓冲区、动画、变量和渲染器等。

<!-- ## Timing Model -->
## 时序模型

<!-- TODO: Talk about scheduling frames, presentation timestamps, etc. -->
TODO：讨论帧调度，演示时间戳等。

<!-- ## Fences -->
## 栅栏
<!-- TODO: Talk about synchronization. -->
TODO：讨论同步。

<!-- # API Guide -->
# API指南

## TODO

<!-- Talk about how to get started using Scenic, running examples,
recommended implementation strategies, etc. -->
讨论如何上手使用Scenic，运行示例和推荐的实现策略等。

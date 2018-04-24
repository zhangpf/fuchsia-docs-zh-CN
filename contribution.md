# 协作方式

-------------------------------------

因为Fuchsia文档现处于频繁变更的阶段，为了保证中文文档和原文内容的一致性和质量，我们在翻译的过程中保持以下的原则：

* 在所有的Markdown文档的标题下方，以如下方式添加英文原文url，该url具体指向其在github镜像上某个变更Id（commit)下的地址。并在更新文档内容的同时，也将一并更新该变更Id：

    ```
    # File Title

    ----

    [*英文原文快照*](https://github.com/fuchsia-mirror/${reposity_name}/blob/${commit_id}/file/path/name.md)

    ----
    ```

* 所有的翻译文档中，其英文原文Markdown注释的方式放置于每段相应中文内容之上，便于其他协作者持续在此基础上优化翻译。Markdown注释并不会被渲染，所以不影响阅读体验：

```
......
<!--- 
Currently there are some temporary syscalls that have been used for early bringup work, 
which will be going away in the future as the long term syscall API/ABI surface is finalized. The expectation is that 
there will be about 100 syscalls.
--->
当前，有一些用于项目早期孵化的临时系统调用，随着长期支持的调用API/ABI稳定
下来，在未来将会被剔除。到时预期会有100个左右的系统调用。

<!---
Zircon syscalls are generally non-blocking. 
The wait_one, wait_many port_wait and thread sleep being the notable exceptions.
--->
Zircon的系统调用通常是非阻塞方式的（non-blocking），但值得一提的是，
`wait_one`，`wait_many`，`port_wait`和线程休眠则是例外。

......

```
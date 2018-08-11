# Escher
---

[*英文原文快照*](https://github.com/fuchsia-mirror/garnet/blob/f33fdaac3eb5311b17a391f0430de029d4b23445/public/lib/escher/README.md)

---
<!-- Escher is a physically based renderer. -->
Escher是一个基于物理的渲染器。

<!-- ## Features -->
## 特征

 <!-- * Volumetric soft shadows
 * Color bleeding
 * Light diffusion
 * Lens effect -->
  
 * 体积柔和阴影
 * 颜色出血
 * 光扩散
 * 镜头效果

<!-- ## Building for Fuchsia -->
## 为Fuchsia而构建

<!-- Escher is part of the default Fuchsia build.  The "waterfall" demo is installed
as `system/bin/waterfall`. -->
Escher是默认构建Fuchsia版本的一部分，`waterfall`演示程序安装在`system/bin/waterfall`路径下。

<!-- ## Building for Linux -->
## 为Linux构建
<!-- Escher can also build on Linux.  In order to do so, you need to: -->
Escher也可以在Linux上构建。为此，你需要：
  <!-- * add the Jiri "escher_linux_dev" manifest, then Jiri update -->
  * 添加jiri的`escher_linux_dev`清单，然后执行`jiri update`
    ```
    cd $FUCHSIA_DIR
    jiri import escher_linux_dev https://fuchsia.googlesource.com/manifest
    jiri update
    ```
    <!-- * as part of the jiri update, Escher will download a private copy of the
      Vulkan SDK to $FUCHSIA_DIR/garnet/public/lib/escher/third_party/vulkansdk/ -->
    * 作为`jiri update`的一部分，Escher将私有的Vulkan SDK副本下载到`$FUCHSIA_DIR/garnet/public/lib/escher/third_party/ vulkansdk/`目录下
    

  <!-- * install build dependencies -->
  * 安装构建依赖项
    ```
    sudo apt install libxinerama-dev libxrandr-dev libxcursor-dev libx11-xcb-dev \
    libx11-dev mesa-common-dev
    ```
  <!-- * install a GPU driver that supports Vulkan -->
  * 安装支持Vulkan的GPU驱动
    * NVIDIA: version >= 367.35
      ```
      sudo apt install nvidia-driver
      ```
    * Intel: Mesa >= 12.0
      ```
      sudo apt install mesa-vulkan-drivers
      ```
  <!-- * set the VK_LAYER_PATH, and LD_LIBRARY_PATH environment variables, e.g.: -->
  * 设置`VK_LAYER_PATH`和`LD_LIBRARY_PATH`环境变量，例如：
    ```
    export VULKAN_SDK=$FUCHSIA_DIR/garnet/public/lib/escher/third_party/vulkansdk/x86_64
    export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$VULKAN_SDK/lib
    export VK_LAYER_PATH=$VULKAN_SDK/etc/explicit_layer.d
    ```
  <!-- * specify that you want to build only Escher (+ examples/tests), for Linux: -->
  * 指定你只想为Linux构建Escher（以及它的examples/tests），例如:
    ```
    cd $FUCHSIA_DIR
    fx set x64 --packages garnet/packages/experimental/disabled/dev_escher_linux --args escher_use_null_vulkan_config_on_host=false
    ```
    <!-- * See `$FUCHSIA_DIR/docs/getting_source.md` for how to set up the `fx` tool. -->
    * 有关如何设置`fx`工具，请参阅 [`$FUCHSIA_DIR/docs/getting_source.md`（*译者注：应该是在getting_started.md文档下的`fx`文档*）](/docs/getting_started.md)。
  <!-- * Do this once only (then you can skip to the next step for iterative development): -->
  * 执行一次（然后可以在迭代开发中跳过此步骤）：
    ```
    fx full-build
    ```
  <!-- * BUILD!! AND RUN!!! -->
  * 构建！！然后运行！！！
    ```
    buildtools/ninja -C out/x64/ && out/x64/host_x64/waterfall
    ```

<!-- Bluetooth -->
蓝牙
=========
---

[*英文原文快照*](https://github.com/fuchsia-mirror/garnet/blob/b104010dcf193b89133c4433560d93c76ba669f0/bin/bluetooth/README.md)

---

<!-- The Fuchsia Bluetooth system aims to provide a dual-mode implementation of the
Bluetooth Host Subsystem (5.0+) supporting a framework for developing Low Energy
and Traditional profiles. -->

Fuchsia的蓝牙系统旨在提供蓝牙主机子系统(5.0+)的双模实现，提供支持开发低功耗和传统配置文件的框架。

<!-- Source code shortcuts: -->
源代码快速链接：
<!-- - [Public API](../../public/lib/bluetooth/fidl)
- [Private API](../../lib/bluetooth/fidl)
- [Tools](tools/)
- [Host Library](../../drivers/bluetooth/lib)
- [Host Bus Driver](../../drivers/bluetooth/host)
- [HCI Drivers](../../drivers/bluetooth/hci)
- [HCI Transport Drivers](https://fuchsia.googlesource.com/zircon/+/master/system/dev/bluetooth?autodive=0) -->
- [公有API](https://github.com/fuchsia-mirror/garnet/tree/master/public/fidl/fuchsia.bluetooth)
- [私有API](https://github.com/fuchsia-mirror/garnet/tree/master/lib/bluetooth/fidl)
- [工具](https://github.com/fuchsia-mirror/garnet/tree/master/bin/bluetooth/tools)
- [主机库](https://github.com/fuchsia-mirror/garnet/tree/master/drivers/bluetooth/lib)
- [主机驱动程序](https://github.com/fuchsia-mirror/garnet/tree/master/drivers/bluetooth/host)
- [HCI驱动](https://github.com/fuchsia-mirror/garnet/tree/master/drivers/bluetooth/hci)
- [HCI传输驱动](https://github.com/fuchsia-mirror/zircon/tree/master/system/dev/bluetooth)
<!-- For more orientation, see -->
有关更多方面的内容，请参阅
<!-- - [System Architecture](../../docs/bluetooth_architecture.md)
- [Detailed Source Layout](../../docs/bluetooth_source_layout.md) -->
- [系统架构（英文原文）](https://github.com/fuchsia-mirror/garnet/blob/master/docs/bluetooth_architecture.md)
- [详细的源码布局（英文原文）](https://github.com/fuchsia-mirror/garnet/blob/master/docs/bluetooth_source_layout.md)

<!-- ## Getting Started -->
## 入门
<!-- ### API Examples -->
### API示例

<!-- Examples using Fuchsia's Bluetooth Low Energy APIs for all four LE roles can be
found in Garnet and Topaz. All of these are currently compiled into Fuchsia by
default. -->
在Garnet和Topaz中，可以找到使用Fuchsia蓝牙低功耗API来处理所有四种LE角色的示例，目前所有这些示例程序都默认编译到Fuchsia中。
<!-- 
- __LE scanner__: see [`eddystone_agent`](https://fuchsia.googlesource.com/topaz/+/master/examples/eddystone_agent/).
This is a suggestion agent that proposes URL links that are obtained from
Eddystone beacons. This is built in topaz by default. -->
- __LE scanner__：请参见[`eddystone_agent`](https://github.com/fuchsia-mirror/topaz/tree/master/examples/eddystone_agent)。这是一个从Eddystone信标处获得的URL链接的建议代理，在默认情况下，它内置于topaz中。
<!-- - __LE broadcaster__: see [`eddystone_advertiser`](https://fuchsia.googlesource.com/topaz/+/master/examples/bluetooth/eddystone_advertiser/).
This is a Flutter module that can advertise any entered URL as an Eddystone
beacon. -->
- __LE broadcaster__：请参见[`eddystone_advertiser`](https://github.com/fuchsia-mirror/topaz/tree/master/examples/bluetooth/eddystone_advertiser)。这是一个Flutter模块，它可以将任何输入的URL作为Eddystone信标进行广播。
<!-- - __LE peripheral__: see the [`ble_rect`](https://fuchsia.googlesource.com/topaz/+/master/examples/bluetooth/ble_rect/)
and [`ble_battery_service`](../../examples/bluetooth/ble_battery_service) examples. -->
- __LE peripheral__：请参见[`ble_rect`](https://github.com/fuchsia-mirror/topaz/tree/master/examples/bluetooth/ble_rect)和[`ble_battery_service`](https://github.com/fuchsia-mirror/garnet/tree/master/examples/bluetooth/ble_battery_service/)示例代码。
<!-- - __LE central__: see [`ble_scanner`](https://fuchsia.googlesource.com/topaz/+/master/examples/bluetooth/ble_scanner/). -->
- __LE central__：请参见[`ble_scanner`](https://github.com/fuchsia-mirror/topaz/tree/master/examples/bluetooth/ble_scanner)。

<!-- ### Control API -->
### 控制API

<!-- Dual-mode (LE + Classic) GAP operations that are typically exposed to privileged
clients are performed using the [control.fidl](../../public/fidl/fuchsia.bluetooth.control.fidl)
API. This API is intended for managing local adapters, device discovery & discoverability,
pairing/bonding, and other settings. -->
通过使用[control.fidl](https://github.com/fuchsia-mirror/garnet/tree/master/public/fidl/fuchsia.bluetooth.control/) API，来执行通常向特权客户端公开的双模(LE + Classic) GAP操作。此API用于管理本地适配器，设备发现和可发现性的配对/绑定以及其他设置。

<!-- [`bt-cli`](tools/bt-cli) is a command-line front-end
for this API: -->
[`bt-cli`](https://github.com/fuchsia-mirror/garnet/tree/master/bin/bluetooth/tools/bt-cli)是该API的命令行前端：


```
$ bt-cli
bluetooth> list-adapters
  Adapter 0
    id: bf004a8b-d691-4298-8c79-130b83e047a1
    address: 00:1A:7D:DA:0A
bluetooth>
```

<!-- We also have a Flutter [module](https://fuchsia.googlesource.com/docs/+/HEAD/glossary.md#module)
that acts as a Bluetooth system menu based on this API at
[topaz/app/bluetooth_settings](https://fuchsia.googlesource.com/topaz/+/master/app/bluetooth_settings/). -->

我们还提供了一个Flutter[模块](/docs/glossary.md#module)，该模块在[topaz/app/bluetooth_settings](https://github.com/fuchsia-mirror/topaz/tree/master/app/bluetooth_settings)上充当基于此API的蓝牙系统菜单。

<!-- ### Tools -->
### 工具

<!-- See the [bluetooth/tools](tools/) package for more information on
available command line tools for testing/debugging. -->
有关用于测试/调试的命令行工具的更多信息，请参见[bluetooth/tools](https://github.com/fuchsia-mirror/garnet/tree/master/bin/bluetooth/tools)程序包。

<!-- ### Running Tests -->
### 运行测试

<!-- #### Unit tests -->
#### 单元测试
<!-- 
The `bluetooth_tests` package contains Bluetooth test binaries. This package is
defined in the [tests BUILD file](tests/BUILD.gn). -->

`bluetooth_tests`程序包中包含蓝牙测试的二进制文件。该程序包在[tests模块的BUILD文件](https://github.com/fuchsia-mirror/garnet/blob/master/bin/bluetooth/tests/BUILD.gn)中定义。

<!--
Host subsystem tests are compiled into a single [GoogleTest](https://github.com/google/googletest) binary,
which gets installed at `/system/test/bluetooth_unittests`. -->

而主机子系统的测试代码被编译为单个[GoogleTest](https://github.com/google/googletest)二进制文件，该文件安装在`/system/test/bluetooth_unittests`目录中。

<!-- ##### Running on real hardware -->
##### 在真实硬件上运行

<!-- * Run all the tests: -->
* 运行所有测试：
  ```
  $ runtests -t bt-host-unittests
  ```


<!-- * Or use the `--gtest_filter`
[flag](https://github.com/google/googletest/blob/master/googletest/docs/advanced.md#running-a-subset-of-the-tests) to run a subset of the tests: -->
* 或者使用`--gtest_filter`[标识](https://github.com/google/googletest/blob/master/googletest/docs/advanced.md#running-a-subset-of-the-tests)运行部分测试：

  <!-- ```
  # This only runs the L2CAP unit tests.
  $ /pkgfs/packages/bluetooth_tests/0/test/bt-host-unittests --gtest_filter=L2CAP_*
  ``` -->
  ```
  # 仅运行L2CAP单元测试。
  $ /pkgfs/packages/bluetooth_tests/0/test/bt-host-unittests --gtest_filter=L2CAP_*
  ```
  <!-- (We specify the full path in this case, because runtests doesn't allow us to pass through arbitrary arguments to the test binary.) -->
  （在这种情况下，我们指定了完整的路径，因为runtests不允许我们传递给测试二进制文件任意参数。）

<!-- * And use the `--verbose` flag to set log verbosity: -->
* 并使用`--verbose`标志设置log的详细程度：

  <!-- ```
  # This logs all messages logged using FXL_VLOG (up to level 2)
  $ /pkgfs/packages/bluetooth_tests/0/test/bt-host-unittests --verbose=2
  ``` -->
  ```
  # 打印使用FXL_VLOG（level 2及其以上）的所有消息
  $ /pkgfs/packages/bluetooth_tests/0/test/bt-host-unittests --verbose=2
  ```
<!-- ##### Running on QEMU -->
##### 在QEMU上运行

<!-- If you don't have physical hardware available, you can run the tests in QEMU using the same commands as above. A couple of tips will help run the tests a little more quickly. -->
如果你没有可用的物理设备，那么可以使用与上面相同的命令在QEMU中运行测试。一些提示将有助于更快地运行测试。

<!-- * Run the VM with hardware virtualization support: `fx run -k` -->
* 运行具有硬件虚拟化支持的虚拟机：`fx run -k`
<!-- * Disable unnecessary logging for the tests: -->
* 禁用测试中不必要的日志记录：
  ```
  $ /pkgfs/packages/bluetooth_tests/0/test/bt-host-unittests --quiet=10
  ```

<!-- With these two tips, the full bt-host-unittests suite runs in ~2 seconds. -->
如果使用这两个提示，完整的`bt-host-unittests`套件可在约2秒内完成运行。

<!-- #### Integration Tests -->
#### 集成测试

<!-- TODO(armansito): Describe integration tests -->
TODO(armansito): 描述集成测试

<!-- ### Controlling Log Verbosity -->
### 控制日志的详细程度

#### bin/bt-gap
<!-- 
The Bluetooth system service is invoked by sysmgr to resolve service requests.
The mapping between environment service names and their handlers is defined in
[bin/sysmgr/config/services.config](../../bin/sysmgr/config/services.config).
Add the `--verbose` option to the Bluetooth entries to increase verbosity, for
example: -->
sysmgr调用蓝牙系统服务来解析服务请求，环境服务名称与其处理服务程序之间的映射在[bin/sysmgr/config/services.config](https://github.com/fuchsia-mirror/garnet/blob/master/bin/sysmgr/config/services.config)中定义。将"--verbose"选项添加到蓝牙项可以增加日志详细程度，例如：

```
...
    "bluetooth::control::AdapterManager": [ "bluetooth", "--verbose=2" ],
    "bluetooth::gatt::Server": [ "bluetooth", "--verbose=2" ],
    "bluetooth::low_energy::Central": [ "bluetooth", "--verbose=2" ],
    "bluetooth::low_energy::Peripheral": [ "bluetooth", "--verbose=2" ],
...

```

#### bthost
<!-- 
The bthost driver currently uses the FXL logging system. To enable maximum log
verbosity, set the `BT_DEBUG` macro to `1` in [drivers/bluetooth/host/driver.cc](../../drivers/bluetooth/host/driver.cc). -->
bthost驱动程序目前使用FXL日志系统。如果要启用最大日志详细程度，请在[drivers/bluetooth/host/driver.cc](https://github.com/fuchsia-mirror/garnet/blob/master/drivers/bluetooth/host/driver.cc)中将`BT_DEBUG`宏设置为`1`。
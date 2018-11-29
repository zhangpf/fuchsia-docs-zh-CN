<!-- # Block device testing -->
# 块设备测试

----

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/863d23444417438d236853b659335cb6558ff76a/docs/block_device_testing.md)

----

<!-- 
__WARNING: All of the following tests are , and they may not
ask for confirmation before executing. Run at your own risk.__ 
-->
__警告：以下所有测试项都有破坏性，并且不会提示确认。运行风险自负。__

<!-- ## Protocol testing -->
## 协议测试

<!-- 
*blktest* is an integration which may be used to check adherence to the block protocol. 
-->
*blktest* 是一个集成的命令，可用于测试块设备通讯协议完整性。

```shell
$ blktest -d /dev/class/block/000
```

<!-- ## Filesystem testing -->
##  文件系统测试

<!-- 
*fs-test* is a filesystem integration test suite that can be used to verify
Fuchsia filesystem correctness on a filesystem. 
-->
*fs-test* 是一个文件系统件集成的测试套件，可用于验证Fuchsia文件系统的正确性。

<!-- 
To avoid racing with the auto-mounter, it is recommended to run this
test with the kernel command line option "zircon.system.disable-automount=true". 
-->
为避免自动文件系统挂载带来的竞争问题，推荐在内核命令行使用"zircon.system.disable-automount=true"配置项运行此命令。

<!-- 
TODO(ZX-1604): Ensure this filesystem test suite can execute on large
partitions. It is currently recommended to use this test on a 1-2 GB GPT
partition on the block device. 
-->
TODO(ZX-1604)：确保此测试套件在大分区文件系统上运行。目前推荐在1-2GB GPT分区的块设备上运行。

```shell
$ /boot/test/fs/fs-test -d /dev/class/block/000 -f minfs
```

<!-- ## Correctness testing -->
## 数据正确性测试

<!-- 
*iochk* is a tool which pseudorandomly reads and writes to a block device to check for errors. 
-->
*iochk* 是一种伪随机读写块设备以检查错误的工具。

```shell
$ iochk -bs 32k -t 8 /dev/class/block/000
```

<!-- ## Performance testing -->
## 性能试验

<!-- 
*iotime* is a benchmarking tool which tests the read and write performance of block devices. 
-->
*iotime* 是一个测试块设备读写性能的基准测试工具。

```shell
$ iotime read fifo /dev/class/block/000 64m 4k
```



<!-- # Fuchsia's libc -->
# Fuchsia的libc
---

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/8ac2b7fda3d21928c5361c896a8a9fab1f7b66a7/docs/libc.md)

---
<!-- TODO(ZX-1598) Type more here. -->
TODO(ZX-1598)：于此输入更多内容。

<!-- ## Standards -->
## 标准

### C11
<!-- 
Fuchsia's libc supports most of the [C11][c11std] standard. This
in particular includes the atomic and threading portions of the
standard library. -->
Fuchsia的libc支持大多数[C11][c11std]标准，尤其包括标准库的原子操作和线程部分。

### POSIX

<!-- Fuchsia implements a subset of POSIX. -->
Fuchsia实现了POSIX的子集。

<!-- Things at least partially supported include the basics of POSIX I/O
(open/close/read/write/stat/...), and pthreads (threads and mutexes). -->
至少部分支持的特性包括POSIX的I/O基础部分(open/close/read/write/stat/...)以及pthreads（线程和互斥）。

<!-- On Fuchsia, the portion of file paths beginning with a sequence of
`..` is resolved locally. See [this writeup][dotdot] for more
information. -->
在Fuchsia上，以“..”为前缀的文件路径部分在本地解析，有关详细信息，请参阅[此文章（英文原文）][dotdot]。

<!-- Similarly, symlinks are not supported on Fuchsia. -->
同样地，Fuchsia不支持符号链接。

<!-- Conspicuously not supported are UNIX signals, fork, and exec. -->
标准中明显不支持的包括：UNIX信号，fork和exec。

## FDIO

<!-- Fuchsia's libc does not directly support I/O operations. Instead it
provides weak symbols that another library can override. This is
typically done by [fdio.so][fdio]. -->
Fuchsia的libc不直接支持I/O操作，相反它提供了一组可由其它库覆盖的弱符号，而这通常由[fdio.so][fdio]提供支持。

<!-- ## Linking -->
## 链接

<!-- Statically linking libc is not supported. Everything dynamically links libc.so. -->
Fuchsia不支持静态链接libc，一切符号动态地链接到libc.so上。

<!-- ## Dynamic linking and loading -->
## 动态链接和加载

<!-- libc.so is also the dynamic linker. -->
libc.so同时也是动态链接器。

[c11std]: https://zh.wikipedia.org/wiki/C11
[dotdot]: https://github.com/fuchsia-mirror/docs/blob/master/the-book/dotdot.md
[fdio]: https://github.com/fuchsia-mirror/zircon/tree/master/system/ulib/fdio
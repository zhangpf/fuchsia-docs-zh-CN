<!-- # h2md - Header To Markdown -->
# h2md - 头文件转Markdown文件

----

[*英文原文快照*](https://github.com/fuchsia-mirror/zircon/blob/3719298073ad6adc5491a1f2a169af5293314208/docs/h2md.md)

----

<!-- 
h2md is a simple tool for generating markdown api docs from headers. 
-->
h2md 是一个简单工具用于将头文件转换为 markdown 文件。

<!-- 
It avoids any dependencies and has a very simple line-oriented parser.
Whitespace at the start and end of lines is ignored. 
-->
h2md 没有任何依赖，是一个非常简单的行解析器。行头尾的空白字符将会被忽略。

<!-- 
Lines starting with `//@` are either a directive to h2md or the start of
a chunk of markdown. 
-->
以 `//@` 开头的行，是 h2md 指令，也是 markdown 文件的开始。

<!-- 
Markdown chunks are continued on every following line starting
with `//`.  They are ended by a blank line, or a line of source code. 
-->
markdown 块每行以 `//` 开头，以空行或者源码结束。

<!-- 
A line of source code after a markdown chunk is expected to be a function
or method declaration, which will be terminated (on the same line or
a later line) by a `{` or `;`. It will be presented as a code block. 
-->
markdown 块后紧跟的第一行源码必须是函数或者函数声明，并以（在同一行或下一行） `{` 或 `;` 结束。

<!-- 
Lines starting with `//{` begin a code block, and all following lines
will be code until a line starting with `//}` is observed. 
-->
代码块以 `//{` 开头，并以 `//}`结束，期间不能穿插非代码行。

<!-- 
To start a new document, use a doc directive, like
`//@doc(docs/my-markdown.md)` 
-->
新建一个文件，需要使用文件指令，如：`//@doc(docs/my-markdown.md)` 。

<!-- 
From the start of a doc directive until the next doc directive, any
generated markdown will be sent to the file specified in the directive. 
-->
从一个文件指令开始，产生的所有 markdown 都会写入这个文件，直到新的文件指令出现为止。

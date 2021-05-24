# 使用WebAssembly系统接口

WebAssembly系统接口(简称WASI)是一种“[模块化系统接口](https://wasi.dev/) ”。现在我们已经学习并尝试了导入和导出，我们可以讨论什么是模块化系统接口以及它为什么重要。

正如我们到目前为止多次提到的，“独立的”WebAssembly模块不能访问操作系统。它不能进行网络调用，不能读写标准设备。它甚至不能启动活动，它必须对来自宿主的刺激作出反应。

WASI是一组逻辑相关的导入，提供对主机运行时所执行的系统的一个小子集的安全和隔离的访问。与所有事情一样，您的模块是否允许使用WASI完全取决于主机运行时，甚至在规范中，您的模块可能允许使用哪些函数。

例如，虽然WASI规范允许导入功能，让WebAssembly模块通过[文件描述符](https://en.wikipedia.org/wiki/File_descriptor) 读取和写入文件，但可用描述符集是由运行时提前确定的。这些模块不能简单地读写主机文件系统的任意部分。

一些以WebAssembly为目标的语言完全了解WASI规范，并将您的原生语言调用作为WASI导入链接到文件系统，而不是将它们链接到对应的原始内核/系统调用。Rust就是其中一种语言。
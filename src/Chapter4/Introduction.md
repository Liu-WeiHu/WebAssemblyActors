# 给出本单元的功能和语法点

在本课程的这一点上，您应该对编写和托管WebAssembly模块感到舒适。在完成实验并跟随代码之后，您还应该能够自如地调用客户函数并支持由客户模块调用的导入。能够来回传递数字对于“hello world”示例来说是很好的，但是如果我们想将WebAssembly用于云和其他地方的真实世界用例，我们需要能够做的远不止这些。

本章介绍了WebAssembly的线性内存，以及来宾和宿主如何与它交互。在本章中，我们还将探讨各种可用的选项，用于在客户机和主机之间构建丰富而强大的通信模式。
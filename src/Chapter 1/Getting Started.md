# 需要哪些工具

我们在本章中需要的唯一工具是您最喜欢的文本编辑器和WebAssembly二进制工具包，或wabt(发音为“wabbit”)。wabt工具都是用C/ c++构建的，您可以通过使用C构建它们来安装它们，也可以从二进制发布存档文件安装它们.

我们认为安装工具包最简单的方法是到[发布页面](https://github.com/WebAssembly/wabt/releases) ，下载适合您的操作系统和处理器体系结构的归档文件，然后将二进制文件放在路径中的某个位置。在编写本课程的时候(2021年3月)，最新的wabt版本是1.0.23。

WebAssembly二进制工具包包括以下工具(还有更多，但这些是与本章最相关的):
- [wat2wasm](https://webassembly.github.io/wabt/doc/wat2wasm.1.html)
    
    虽然从技术上讲它不是编译器，但它可以帮助学习过程以这种方式思考。这个工具获取原始的wat代码(我们将在本章中编写)，并从中生成二进制WebAssembly模块。
- [wasm2wat](https://webassembly.github.io/wabt/doc/wasm2wat.1.html)

  与wat2wasm相反，这个工具“反编译”一个二进制WebAssembly模块并生成一个wat文本文件。
- [wasm-objdump](https://webassembly.github.io/wabt/doc/wasm-objdump.1.html)

  打印和查询有关wasm二进制文件内容的信息的极其有用的工具。
- [wasm-interp](https://webassembly.github.io/wabt/doc/wasm-interp.1.html)

  该工具读取和解释wasm二进制文件，允许您调用模块中的函数。
- [wasm-strip](https://webassembly.github.io/wabt/doc/wasm-strip.1.html)
  该工具用于通过剥离各个部分和组件来减少wasm二进制文件的大小。

为了确保您已经安装了工具包，并且它在您的路径中可用，请在终端提示符下发出以下命令:

<font color=Blue>$ wat2wasm --version</font>

<font color=Blue>1.0.13</font>

您的版本可能与上面的不同，但重要的是该工具是可用的，并且它可以在您的机器上运行。注意，您可能会看到单个工具版本的1.0.13，即使您安装了1.0.20版本存档。

如果你打算使用Visual Studio Code，你可以安装一个插件来支持从市场上对wat文件的语法高亮显示。在写这门课程的时候(2021年3月)，有两个可用的扩展:[WebAssembly Syntax (Lite)](https://marketplace.visualstudio.com/items?itemName=jcmellado.vscode-wasm-syntax-lite) 和用于VSCode的[WebAssembly Toolkit](https://marketplace.visualstudio.com/items?itemName=dtsvet.vscode-wasm) 。

还有一个wabt的网络端口，你可以通过npm安装，如果你更习惯使用这些工具和一个JavaScript API。
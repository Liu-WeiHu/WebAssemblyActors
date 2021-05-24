# 创建您的第一个WebAssembly模块(2)

现在让我们看看当我们使用另一个工具来逆转这个过程，并从wasm二进制文件中取回一个wat文件时会发生什么:

<font color=Blue>wasm2wat add.wasm</font>

```text
(module
  (type (;0;) (func (param i32 i32) (result i32)))
  (func (;0;) (type 0) (param i32 i32) (result i32)
     local.get 0
     local.get 1
     i32.add)
  (export "add" (func 0)))
```

代码看起来有点不同，但它基本上是相同的代码，我们开始。有趣的是，你可以看到在第二个机器生成的代码中，函数参数没有命名，它们是通过索引访问的。参数名称的存在只是为了人类手写wat代码的好处。换句话说，如果您将其他人的wasm二进制文件转换为一个wat文件，您将看不到变量名，因此一旦超出几个函数，它们就很难读取。

到目前为止，一切都很好，但是我们如何执行我们在模块中得到的函数呢?我们可以使用JavaScript和浏览器作为快速和简单的辅助工具，但是让我们做一些更“模糊”的东西。我们将安装另一个命令行工具，在许多其他特性中，它允许我们调用wasm文件中的任意函数:wasmtime(稍后您将看到，wasmtime也是一个非常强大的Rust板条箱，用于在Rust中编译和运行WebAssembly模块)。

一旦你遵循了wasmtime的安装说明，你就可以执行我们新编写的add函数，如下所示:

<font color=Blue>wasmtime add.wasm --invoke add 1 2</font>

```text
warning: using `--invoke` with a function that takes arguments is experimental and may break in the future
warning: using `--invoke` with a function that returns values is experimental and may break in the future
3
```

忽略关于未来兼容性的警告，我们看到我们得到了正确的答案(3)。如果这一切看起来太基本和低水平，不要气馁。现在我们正在编写像add这样的函数，但不久我们将编写像handleaccountwith提款或updateShoppingCart这样的东西。
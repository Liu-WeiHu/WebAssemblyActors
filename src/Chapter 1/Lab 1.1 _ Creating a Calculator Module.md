# 实验1.1 -创建一个计算器模块

对于这个实验，您将在本节中编写的简单add模块的基础上进行构建。当它完成时，你应该有一个WebAssembly模块，它导出了以下带有这些类型签名的函数:
- **add(i32, i32) -> i32** 
- **sub(i32, i32) -> i32**
- **mul(i32, i32) -> i32** 
- **div(i32, i32) -> i32**

要记住的关键事情是，您需要在模块的底部为您编写的每个函数设置一个导出行。如果您喜欢这种语法，您还可以指示一个函数正在内联导出。要了解i32数据类型可用哪些函数，请查看这里所有可用操作的列表。本文档有点学术性，所以如果您正在寻找更多实际的wat语法示例，那么"wabbit"工具的[解析测试套件](https://github.com/WebAssembly/wabt/tree/master/test/parse) 是一个很好的起点。您可能还会在这个[wabt测试](https://github.com/WebAssembly/wabt/blob/c9d29f0a26bf2915288d8653258aa4fc89cc8d0f/test/interp/binary.txt) 中找到一些关于导出数学函数的灵感。

在构建实验室时，现在不要担心错误处理。当我们需要担心错误检查的时候，我们将在高级语言中工作。如下所示，当你试图除以0时，你的计算器模块应该崩溃(在WebAssembly中称为“陷阱”):

<font color=Blue>wasmtime calculator.wasm --invoke div 5 0</font>

```text
warning: using `--invoke` with a function that takes arguments is experimental and may break in the future
Error: failed to run main module `calculator.wasm`

Caused by:
     0: failed to invoke `div`
     1: wasm trap: integer divide by zero
     wasm backtrace:
     0: 0x54 - <unknown>!<wasm function 3>
```

作为提示，当您处理div函数导出时，您必须选择是使用有符号除法还是无符号除法，因为对于i32类型没有简单的“div”指令。
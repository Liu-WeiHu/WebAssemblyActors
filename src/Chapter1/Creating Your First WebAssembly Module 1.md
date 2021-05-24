# 创建您的第一个WebAssembly模块(1)

最简单的方法是打开你喜欢的文本编辑器，写一些wat代码。在编辑器中粘贴或键入以下内容:

```webassembly
(module
  (func $add (param $lhs i32) (param $rhs i32) (result i32)
     (i32.add
       (get_local $lhs)
       (get_local $rhs)
     )
  )
  (export "add" (func $add))
)
```

一旦你将文件保存为add.wat，你可以使用wabt工具从中生成一个wasm模块:

<font color=Blue>wat2wasm add.wat</font>

如果没有结果输出，则一切正常(在本例中，没有消息就是好消息)。我们现在应该有一个导出单个添加函数的WebAssembly模块。在我们执行它之前，让我们使用更多的工具包来探索新的add.wasm文件:

<font color=Blue>wasm-objdump -x add.wasm</font>

```text
add.wasm: file format wasm 0x1

Section Details:

Type[1]:
 - type[0] (i32, i32) -> i32
Function[1]:
 - func[0] sig=0 <add>
Export[1]:
 - func[0] <add> -> "add"
Code[1]:
 - func[0] size=7 <add>
```

这个输出告诉我们，add.wasm文件包含单个类型签名(一个函数接受两个i32参数并返回一个i32值)、一个名为add的函数和一个名为“add”的导出。

在下一页继续。
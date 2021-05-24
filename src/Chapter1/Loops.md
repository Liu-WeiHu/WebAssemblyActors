# 循环

循环，就像WebAssembly中的其他所有东西一样，已经被浓缩为它们最简单的表示。我们没有while、until或for语句，也没有列表推导式、流或迭代器。相反，我们可以使用loop语句启动循环，然后可以使用br和br_if退出循环或回到起点。

br和br_if都将标签索引作为第一个参数。这里过于简化了，但是为了继续本节的内容，您可以将“br 0”指令看作等同于“返回到循环顶部”，而将“br 1”看作是“退出循环”。退出循环的索引值将根据你嵌套的深度而改变(这使得在wat中读写嵌套循环非常痛苦)。

希望在这一点上，您正在欣赏这些高级循环语句在您最喜欢的语言中是多么有用。看看这个等价于用wat编写的for循环:

```webassembly
(module
  (func $forLoop (result i32)
      (local $x i32)
      (local $res i32)

      (set_local $x (i32.const 0))
      (set_local $res (i32.const 0))

      (block
        (loop
         (set_local $x (call $increment (get_local $x)))
         (set_local $res (i32.add (get_local $res) (get_local $x)))
         (br_if 1 (i32.eq (get_local $x) (i32.const 20)))
         (br 0)
      )
    )

    (get_local $res)
  )

  (func $increment (param $x i32) (result i32)
     (i32.add (get_local $x) (i32.const 1))
  )

  (export "forLoop" (func $forLoop))
)
```

在前面的代码中，$forLoop函数使用$x本地变量作为循环计数器，并通过添加循环计数器(例如0 + 1 + 2 + 3，等等)逐步建立$res值。当循环计数器等于20时，br_if行将终止循环。

我们可以使用wat2wasm生成二进制文件，并使用wasmtime执行:

<font color=Blue>wat2wasm forloop.wat

wasmtime forloop.wasm --invoke forLoop</font>

```text
warning: using `--invoke` with a function that returns values is experimental and may break in the future
210
```

由于我们负责手动决定何时回到循环的起点，何时终止循环，因此创建while循环、until循环和所有其他形式的循环的模板看起来与前面的for循环的代码完全相同。

如果您对构建循环的所有可能方法都感兴趣，请查看循环指令的[WebAssembly规范测试套件](https://github.com/WebAssembly/testsuite/blob/master/loop.wast) 。
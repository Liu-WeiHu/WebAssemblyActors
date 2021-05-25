# 流程控制

正如我们所熟悉和依赖的复杂数据类型最终归结为少量的数值类型一样，我们最喜欢的控制流结构可以简化为非常少量的指令。我们将在本节讨论这些说明。

作为参考，控制指令列表可以在[说明书](https://webassembly.github.io/spec/core/syntax/instructions.html#syntax-instr-control) 中找到。正如我们前面提到的，许多人发现这种类型的文档很难阅读，所以在野外找到运行示例以查看语法示例可能更容易。

与本章需求最相关的控制指令是:
- **loop** - WebAssembly中唯一的循环指令(没有“for”或“while”指令)WebAssembly中唯一的循环指令(没有“for”或“while”指令)
- **br** - 无条件转移指令
- **br_if** - 条件转移指令
- **if** - 一种可以选择性返回值的条件指令

在WebAssembly中有两种主要的分支方式:用if语句或者显式的分支指令。If语句可以提供true和false分支，这些分支执行有返回值或没有返回值的工作。

不返回值的if语句的格式如下:

```webassembly
(if (condition)
    (then …)
    (else …)
)
```

这里的条件可以是计算结果为布尔值的任何东西，因此它可以是一个表达式块，也可以是单个表达式。下面是一个示例if语句，它根据$row和$col的局部变量是否相等进行分支:

```webassembly
(if
  (i32.eq
    (get_local $row)
    (get_local $col)
  )
  (then
    ...
  )
 (else
   ...
  )
)
```

在前面的代码中，如果 **$row** local(它可以是一个局部作用域变量或函数参数)的值等于 **$col** local，那么将执行" then "下的分支，否则将运行" else "分支。

在许多情况下，我们可能希望使用if语句返回值。我们可以这样写:

```webassembly
(if (result i32)
  (i32.eq
    (get_local $row)
    (get_local $col)
  )
  (then
    (i32.const 42)
  )
  (else
    (i32.const 24)
  )
)
```

注意，在if语句的谓词之前添加了(**result i32**)元素，表示返回值的数据类型。
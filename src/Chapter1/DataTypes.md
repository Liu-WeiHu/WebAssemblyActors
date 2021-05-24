# 数据类型

WebAssembly只有四种数据类型。有些东西被正式分类为类型，比如函数、表和内存，但当涉及到我们可以在编写的函数中声明和操作的变量时，我们只有以下四种类型:
- **i32** - 32-bit integer
- **i64** - 64-bit integer
- **f32** - 32-bit floating point number
- **f64** - 64-bit floating point number

您可能注意到这个列表中遗漏了一件事，特别是如果您曾经使用过像C、Zig和Rust这样的语言，那么它就是无符号数字数据类型的可用性。在声明变量时，我们习惯于声明数字应该被视为有符号还是无符号。在WebAssembly中，我们选择对数字的操作是作为有符号的还是无符号的执行。例如，下面是一个不完整的列表，列出了一些可用于有符号和无符号类型的数字的操作:
- **le_u** and **le_s** - Less than or equal to (<=)
- **ge_u** and **ge_s** - Greater than or equal to (>=)
- **lt_u** and **lt_s** - Less than (<)
- **gt_u** and **gt_s** - Greater than (>)
- **div_s** and **div_u **- Division (/)
- **rem_s** and **rem_u** - Remainder or Modulo

没人会责怪你现在的想法:“只有数字?”我们怎么可能用四种数字类型来构建有用的东西呢?”我们在电脑上所做的一切最终都归结为数字——比特和字节。高级编程语言把结构和类转换成内存中的字节块，把字符串转换成unicode字符值或ASCII字节的序列，这些都给我们带来了麻烦。

除了数字，什么都不做是非常辛苦、乏味和缓慢的。这就是为什么我们将只工作与原始wat这一章。了解“裸”WebAssembly自己能做什么和不能做什么有助于维护透视图，这样您就可以清楚地看到什么是核心WebAssembly规范的一部分，什么是附加工具之间的边界。
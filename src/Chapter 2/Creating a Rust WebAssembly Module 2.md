# 创建一个Rust WebAssembly模块(2)

除了写一个两个数相加的函数，这里还有一些有趣的事情。extern关键字告诉Rust编译器该函数正在被导出。请记住，主机只能调用导出的WebAssembly模块中的函数。对于非webassembly目标，可以使用相同的关键字通过FFI等机制与其他语言库进行互操作性。

Rust编译器非常擅长消除未使用的代码。如果编译这个函数时没有no_mangle属性，那么生成的模块将不包括add函数。这是因为它无法为add函数找到任何消费者，认为这是不必要的，因此从编译器输出中删除了它。有一些其他方法来“诡计”编译器到离开你外面的功能完整,但我们更喜欢no_mangle属性,因为它是明确的,你不希望函数签名篡改,它有自然的副作用,要求被包括在编译输出的函数。

让我们生成一个发布版本(删除所有不必要的调试代码，通常注入到Rust WebAssembly模块)，看看我们得到了什么:

<font color=Blue>cargo build --target wasm32-unknown-unknown --release</font>

使用我们在前一章学到的工具，我们可以检查原始的浪费(文本)格式代码是什么样的:

<font color=Blue>wasm2wat ./target/wasm32-unknown-unknown/release/calc.wasm</font>

```webassembly
(module
  (type (;0;) (func (param i32 i32) (result i32)))
  (func $add (type 0) (param i32 i32) (result i32)
      local.get 1
      local.get 0
      i32.add)
  (table (;0;) 1 1 funcref)
  (memory (;0;) 16)
  (global (;0;) (mut i32) (i32.const 1048576))
  (global (;1;) i32 (i32.const 1048576))
  (global (;2;) i32 (i32.const 1048576))
  (export "memory" (memory 0))
  (export "add" (func $add))
  (export "__data_end" (global 1))
  (export "__heap_base" (global 2)))
```

Rust生成我们之间唯一的区别,我们手工写在最后一章,我们命名变量添加函数和这段代码使用指标,而且你还会注意到,有两个出口全局允许主机知道堆启动和停止。
为了确保这段代码与第一章的加法函数完全相同，让我们再次使用wasmtime:

<font color=Blue>wasmtime ./target/wasm32-unknown-unknown/release/calc.wasm --invoke add 3 5</font>

```text
warning: using −−∈voke with a function that takes arguments is experimental and may break in the future
warning: using −−∈voke with a function that returns values is experimental and may break in the future
8
```

现在，您应该能够使用原始WebAssembly文本格式或基本Rust函数创建基本函数。我们不需要知道内部工作的细节，就可以知道wasm32-unknown-unknown目标正在代表我们将我们的Rust代码转换为WebAssembly字节码。一旦编译后的输出是WebAssembly二进制文件，它就真正具有可移植性，并且与语言无关、与cpu无关、与操作系统无关。
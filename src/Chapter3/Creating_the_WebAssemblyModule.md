# 创建WebAssembly模块

像我们之前所做的那样，创建一个名为importer的新Rust库样式的板条箱。您的Cargo.toml文件应如下所示（显然作者信息会有所不同）：

```toml
[package]
name = "importer"
version = "0.1.0"
authors = ["A Developer"]
edition = "2018"

[lib]
crate-type = ["cdylib"]

[profile.release]
opt-level = "s"

[dependencies]
```

这个项目实际上不需要任何依赖项(预先警告:这是WebAssembly的另一个方面，我们可以在后面的课程中利用它)。接下来，确保您的src/lib。Rs文件如下所示:

```rust
#[link(wasm_import_module = "utilities")]
extern "C" {
     pub fn random() -> i32;
}

#[no_mangle]
extern fn addto(x: i32) -> i32 {
     unsafe {
       x + random()
     }
}
```

这里只有很少几行代码，但是大多数都很重要。我们看到的第一行是Rust的编译器指令，它表明当我们编译到WebAssembly时，应该从实用程序模块导入外部函数random。这里需要注意的是，随机函数没有主体。这是因为它的实现将由主机提供(或者，可能由主机提供的另一个WebAssembly模块提供)。

接下来的几行代码我们之前已经看到过:使用no_mangle来避免更改函数的名称，并使用extern关键字来告诉Rust编译器生成一个名为addto的WebAssembly导出。

接下来，让我们讨论一下不安全关键字。如果你是Rust的新手(或者你错过了我们在前一章所讨论的内容)，这个关键词会让大多数经验丰富的开发者感到恐惧。Rust的激进主张是，它可以保证内存安全，不受竞态条件、缓冲区溢出等的影响。但是，在某些情况下，比如调用外部目标时，编译器不能提供相同的保证。在这种情况下，您应该将不安全的关键字替换为“安全不能保证”。

我们必须将对random的调用封装在一个不安全的块中，但是我们的函数只是返回输入参数x加上主机作为随机调用结果给我们的值。

要构建这个模块的一个小的发布级版本，可以从项目的根目录运行以下命令(当我们构建主机时，下一步需要这个模块):

<font color=Blue>cargo build --release --target wasm32-unknown-unknown</font>


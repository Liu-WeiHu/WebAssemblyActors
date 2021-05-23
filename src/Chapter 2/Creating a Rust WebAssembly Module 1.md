# 创建一个Rust WebAssembly模块(1)

我们通过创建Rust库板条箱来创建WebAssembly模块。为此，我们将使用cargo命令，如下所示:

<font color=Blue>cargo new --lib calc</font>

这将在一个名为calc的目录中创建一个新的Rust库模块，进入该目录并输入以下命令:

<font color=Blue>cargo build --target wasm32-unknown-unknown</font>

这将编译库，但如果您去查找，将在任何地方都找不到.wasm文件。为什么货物没有产生它，为什么它没有抱怨，如果有什么问题?

当您创建项目时，它使用了一个几乎为空的Cargo。toml文件。这是Rust的项目清单，WebAssembly的目标有一个重要的怪癖。默认情况下，Rust将生成一个操作系统本机库(即使您告诉它您想要wasm!)。要获得已编译的wasm文件，需要确保项目的库类型设置为cdylib。使用您最喜欢的编辑器，修改Cargo。Toml文件，它看起来像这样:

```text
[package]
name = "calc"
version = "0.1.0"
authors = ["A Developer"]
edition = "2018"

[lib]
crate-type = ["cdylib"]

[profile.release]
opt-level = "s"

[dependencies]
```

正确设置板条箱类型后，你应该能够再次为wasm32-unknown-unknown目标运行cargo build，然后看到所需的输出:

<font color=Blue>cargo build --target wasm32-unknown-unknown</font>

```text
ll ./target/wasm32-unknown-unknown/debug/
total 1532
drwxrwxr-x 7 kevin kevin 4096 Nov 28 09:56 ./
drwxrwxr-x 4 kevin kevin 4096 Nov 28 09:56 ../
drwxrwxr-x 2 kevin kevin 4096 Nov 28 09:56 build/
-rw-rw-r-- 1 kevin kevin 134 Nov 28 09:56 calc.d
-rwxrwxr-x 2 kevin kevin 1531917 Nov 28 09:56 calc.wasm*
-rw-rw-r-- 1 kevin kevin 0 Nov 28 09:56 .cargo-lock
drwxrwxr-x 2 kevin kevin 4096 Nov 28 09:56 deps/
drwxrwxr-x 2 kevin kevin 4096 Nov 28 09:56 examples/
drwxrwxr-x 3 kevin kevin 4096 Nov 28 09:56 .fingerprint/
drwxrwxr-x 3 kevin kevin 4096 Nov 28 09:56 incremental/
```

您的输出可能会有所不同，但重要的一点是calc.wasm文件现在已经存在，不过由于它是一个调试构建，所以它非常大且膨胀。

现在让我们看一下代码。当你第一次在Rust中创建一个库时，你会得到一个空模块和一个测试子模块，它会做出一个永远不会失败的断言:

```rust
#[cfg(test)]
mod tests {
     #[test]
     fn it_works() {
     assert_eq!(2 + 2, 4);
     }
}
```

正如我们在前一章所做的，我们将开始创建一个函数，两个数相加。替换src/lib中的上述代码。载有下列文件的档案:

```rust
#[no_mangle]
extern fn add(x: i32, y: i32) -> i32 {
     x + y
}
```

在下一页继续。
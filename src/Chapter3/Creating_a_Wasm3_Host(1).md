# 创建一个Wasm3主机(1)

在Rust中创建wasm3运行时主机涉及以下步骤:
- 创建一个Environment实例
- 从环境中创建一个Runtime实例
- 解析WebAssembly模块的字节以生成ParsedModule
- 从ParsedModule创建一个模块实例。
- 使用模块实例来查找和调用导出的函数

wasm3 Rust库实际上是C库的外部函数接口(FFI)包装。通常这种箱是我们鼓励初学者避免,但是你会看到这种类型的东西出现在这么多箱参与WebAssembly生态系统,在这种情况下,它可能是更好的预先处理它,俗话说的好,“早期撕掉这个创可贴”。

在编译使用wasm3的Rust项目之前，您需要在机器上安装Clang 9或更高版本及其所有相关工具。您可以选择按照说明在本地构建Clang，也可以使用您喜欢的包管理器安装Clang，或者下载一个官方版本。虽然我们不能做出任何承诺，但我们相信，安装Clang可能是整个过程中最令人沮丧或阻力最大的部分，这取决于你的操作系统。

在shell中发出以下命令创建一个新的Rust二进制/命令行项目:

<font color=Blue>cargo new rust-host</font>

```text
Created binary (application) `rust-host` package
```

现在可以进入新目录(rust-host)并打开Cargo。Toml与您最喜欢的文本编辑器，添加以下依赖:

```toml
[dependencies]
wasm3 = {version = "0.1.3", features = ["build-bindgen"]}
```

build-bindgen特性指示wasm3使用bindgen工具的嵌入式版本。如果我们不启用这个特性，我们还必须安装bindgen命令行工具。Bindgen是一个库(以及附带的工具)，用于创建C或c++风格头文件的FFI包装器。

正如您可能已经猜到的，wasm3在内部使用它来创建规范C库周围的Rust包装函数和数据类型。

在下一页继续。
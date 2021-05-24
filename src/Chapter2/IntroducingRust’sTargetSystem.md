# 介绍Rust的目标系统

Rust目标是将Rust代码编译为可以在特定操作系统和CPU体系结构上运行的本机二进制代码的方法。默认情况下，Rust编译器是一个跨平台编译器(尽管对于某些类型的目标，您可能需要额外的工具)。在下一节中您将看到，Rust可用的两个目标是WebAssembly目标。

要查看目标系统的运行情况，运行以下命令(此时应该安装了Rust和rustup命令):

<font color=Blue>rustup target list</font>

```text
aarch64-apple-ios
aarch64-linux-android
aarch64-pc-windows-msvc
aarch64-unknown-linux-gnu
aarch64-unknown-linux-musl
arm-unknown-linux-gnueabi
armv7-unknown-linux-gnueabihf (installed)
i686-unknown-linux-gnu
mips-unknown-linux-gnu
mips-unknown-linux-musl
powerpc-unknown-linux-gnu
thumbv7em-none-eabihf
thumbv7m-none-eabi
wasm32-unknown-emscripten (installed)
wasm32-unknown-unknown (installed)
wasm32-wasi (installed)
x86_64-apple-darwin
x86_64-apple-ios
x86_64-unknown-linux-gnu (installed)
x86_64-unknown-linux-musl (installed)
```

对于这个列表，为了简洁起见，我们删除了许多项。显示为“已安装”的目标列表将根据您的操作系统、CPU架构和所做的工作而有所不同。你应该从这个列表中了解到的是，你可以安装多个目标，它们不一定要匹配你的CPU+OS配置。

默认情况下，您自己的操作系统和CPU组合的目标已经安装，您将看到已安装的目标在上述命令的输出中显示。

本章我们将使用两个不同的WebAssembly目标。执行如下命令安装:

<font color=Blue>rustup target add wasm32-unknown-unknown

rustup target add wasm32-wasi</font>

我们将在本章的其余部分讨论这两个目标。
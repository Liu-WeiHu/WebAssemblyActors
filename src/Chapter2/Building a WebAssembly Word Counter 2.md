# 构建一个WebAssembly单词计数器(2)

首先，让我们运行这个项目，就像它是一个普通的Rust项目，而不是WebAssembly:

<font color=Blue>cargo run -- ~/Code/tmp/LICENSE</font>

```text
Finished dev [unoptimized + debuginfo] target(s) in 0.00s
     Running `target/debug/wordcounter /home/kevin/Code/tmp/LICENSE`
 1
1 2
2 1
2.0 2
2004 1
3 1
4 1
5 1
50 1
6 1
7 1
8 1
9 2
a 22
above 1
acceptance 1
accepting 3
act 1
acting 1
acts 1
add 2
addendum 1
additional 5
additions 1
...
```

这里我们只是将路径传递给一个ASCII文本文件作为程序的唯一参数。在前面的例子中，它是Apache-2许可文本，但是您可以随意提供任何文本文件的路径。正如您在输出中所看到的，在开头有一些条目显然不是单词，如果我们的目标是创建一个生产级单词计数器，那么就应该排除它们。

现在让我们将这个应用程序编译为WASI模块，看看能否从wasmtime运行它。首先让我们将它编译到wasm32-wasi目标。如果还没有安装此目标，则需要运行rustup target add wasm32-wasi使其可用，如本章前面所述。

<font color=Blue>cargo build --target wasm32-wasi</font>

```text
Finished dev [unoptimized + debuginfo] target(s) in 0.00s
```

我们现在应该有一个名为wordcounter的文件。Wasm在target/wasm32-wasi/debug目录下。让我们看看wasmtime能否执行它。记住，我们需要告诉wasmtime我们允许模块访问哪个目录。如果没有这个权限，模块将无法读取输入文件:

<font color=Blue>wasmtime --dir=/home/kevin/Code/ target/wasm32-wasi/debug/wordcounter.wasm ~/Code/tmp/LICENSE</font>

```text
WARN wasmtime::linker > command module exporting '__data_end' is deprecated
 WARN wasmtime::linker > command module exporting '__heap_base' is deprecated
 1
1 2
2 1
2.0 2
2004 1
3 1
4 1
5 1
50 1
6 1
7 1
8 1
9 2
a 22
above 1
acceptance 1
accepting 3
act 1
acting 1
acts 1
add 2
...
```

您可能看不到wasmtime发出的警告。它基本上告诉我们，Rust编译了一些东西到我们的WebAssembly模块中，这些东西目前是不赞成的。在你上这门课的时候，这个问题可能已经在Rust版本更新中解决了。

好消息是，您现在知道了如何构建一个可以像普通二进制文件那样运行的应用程序，并且可以在wasmtime或wasmer等支持wasi的运行时的监督下运行它。

但是请注意——之所以能够以这种方式运行它，是因为您的应用程序只使用WASI支持的可用系统调用的有限子集。一旦偏离了这一点，应用程序可能会在终端提示符下正常运行，但无法编译或作为WASI模块运行。
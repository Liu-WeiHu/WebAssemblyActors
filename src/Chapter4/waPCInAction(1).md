# 运行中的waPC(1)

waPC板条箱将协议封装在一组方便的包装器中，因此作为开发人员的您不必担心底层的管道。即使你不需要每天接触管道，你也应该知道它是如何工作的，这样你就可以欣赏为你做的工作。

我们将要构建的示例将是我们之前构建的问候演示的一个变体，以说明线性内存的使用。主机将在客户机上调用一个名为SayHello的操作，该操作向客户机发送一个名称。接着，来宾将调用一个名为GetGreeting的操作，该操作向主机请求问候语字符串。

正如您可以想象的那样，如果没有围绕传递不透明二进制数据的合约进行抽象，这种双向通信将很快变得非常糟糕。让我们从WebAssembly来宾模块开始，您可以在wapc/helloguest目录中将其创建为标准的Rust库。在wapc-guest板条箱上添加一个依赖项，在本课程编写的时候，它是在0.4.0版本上的。

你的src/lib.rs文件应该如下所示:

```rust
extern crate wapc_guest as guest;

use guest::prelude::*;

#[no_mangle]
pub extern "C" fn wapc_init() {
  register_function("SayHello", do_hello);
}

fn do_hello(msg: &[u8]) -> CallResult {
      let name = std::str::from_utf8(msg)?;
      let res =
      host_call("default",
                 "sample",
                 "GetGreeting",
                 &vec![])?;
      let greeting = std::str::from_utf8(&res)?;
      let output = format!("{}, {}!", greeting, name);

      Ok(output.as_bytes().to_vec())
}
```

在下一页继续。
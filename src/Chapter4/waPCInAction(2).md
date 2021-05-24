# 运行中的waPC(2)

首先，我们导出一个名为wapc_init的函数，该函数由主机箱调用(稍后将看到)。在这个函数中，我们注册了操作处理程序。在我们的例子中，我们为一个名为SayHello的操作注册了一个处理函数。

在SayHello处理程序中，我们从有效负载中提取名称，然后通过调用GetGreeting操作向主机请求问候字符串。最后，我们返回一个格式化的欢迎字符串作为函数的返回值。

你(谢天谢地)没有看到的是这个交换必须发生的所有潜在的线性内存操作。

创建此模块的发布版本:

<font color=Blue>cargo build --target wasm32-unknown-unknown --release</font>

现在，让我们在wapc/hellohost目录中创建一个新的rust板条箱。确认货物。Toml有以下依赖关系:

```toml
[dependencies]
wasmtime-provider = "0.0.2"
wapc = "0.10.1"
```

引擎的选择是我们可以在设计时做出的，允许我们在wasmtime这样的jit引擎和wasm3这样的解释引擎之间进行选择。让我们来看看这个主机的主要功能:

```rust
extern crate wapc;
use std::error::Error;
use wapc::WapcHost;
use wasmtime_provider::WasmtimeEngineProvider;

pub fn main() -> Result<(), Box<dyn Error + Send + Sync>> {
  let module = load_file()?;
  let engine = WasmtimeEngineProvider::new(&module, None);
  let host = WapcHost::new(Box::new(engine), move |_id, bd, ns, op, payload| {
    handle_callback(bd, ns, op, payload)
    })?;

  let res = host.call("SayHello", b"Alice")?;
  let s = std::str::from_utf8(&res)?;
  println!("{}", s);
  assert_eq!(s, "Ahoy There, Alice!");
  Ok(())
}
```

在下一页继续。
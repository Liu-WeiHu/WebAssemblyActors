# Actor Ping Pong (1)

在这个示例中，我们将演示两个参与者之间可以进行的最简单和最基本的通信:ping。可以将ping-pong例子看作是“actor模型的hello world”。在任何ping-pong样本中，我们都需要以下东西:
- A pinger - an actor 发送一个ping消息
- A ponger - an actor 响应ping消息
- The ping - 两个actors都能理解的数据结构

让我们先看看ping。这是一个简单的数据结构，但这里发生了一些微妙的事情。首先，wasmCloud使用[消息包](https://msgpack.org/index.html) 来序列化和反序列化参与者和功能之间的消息。如果您曾经使用过协议缓冲区、Avro或无数其他消息序列化方案，那么您就会知道，这些方案通常伴随着代码生成，生成读取和写入给定形状的数据所需的正确代码。我们的Ping结构没有什么不同，需要在发送到参与者的过程中进行序列化，并由接收方进行反序列化。

下面是我们将要使用的ping-pong类型:

```rust
#[derive(Debug, PartialEq, Deserialize, Serialize, Default, Clone)]
pub struct Ping {
     #[serde(rename = "value")]
     pub value: i32,
}

#[derive(Debug, PartialEq, Deserialize, Serialize, Default, Clone)]
pub struct Pong {
     #[serde(rename = "value")]
     pub value: i32,
}
```

如果这里使用的注释看起来不熟悉，不要担心。[Serde](https://crates.io/crates/serde) 可能是Rust社区中最流行的序列化框架，所以如果您正在使用Rust，那么它将随处可见。我们还需要更多的包装和连接组织来让我们的角色相互交谈，但这已经由waPC命令行界面和代码生成器生成。您可以在这个[Github存储库](https://github.com/wasmCloud/examples/blob/main/actor-to-actor/ping-interface/src/generated.rs) 中看到生成的结果。

接下来，让我们看看接收ping消息的参与者:

```rust
use crate wapc_guest as guest;
use ping_interface as demo;
use wasmcloud_actor_core as actor;

use guest::prelude::*;

#[actor::init]
fn init() {
      demo::Handlers::register_ping(handle_ping);
}

fn handle_ping(ping: demo::Ping) -> HandlerResult<demo::Pong> {
     Ok(demo::Pong {
         value: ping.value + 42,
        }
     )
}
```

这段代码的重要之处在于，您只是返回一个值。这段代码本身并不关心如何返回值，只关心返回什么。使用wasmCloud，这种乒乓交换可以在单个进程内本地发生，也可以在云中发生，或者跨多个云，甚至在云和浏览器之间发生。

现在是有趣的事情——让一个演员给另一个演员打电话。为了让一个参与者与另一个参与者交谈，调用者需要知道另一个参与者的身份。这可以是该参与者的公钥(在wasmCloud中，它是一个56个字符的字符串，以字母M(表示“module”)开头)，也可以是一个更人性化的名称。wasmCloud中的参与者可以声明调用别名，从而允许使用别名调用它们。这个调用别名是使用洗涤命令行工具创建的，用来给一个参与者签名，就像我们在前一章中所做的那样。

在下一页继续。

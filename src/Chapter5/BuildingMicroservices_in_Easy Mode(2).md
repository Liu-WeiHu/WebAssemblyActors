# 以“简单模式”构建微服务(2)

wasmCloud有模板和脚手架生成器，可以让创建角色变得更容易，但是我们将在这篇介绍中从头开始，这样你就可以看到所有东西是如何工作的。首先，创建一个名为playervault的新库箱。确认货物。Toml的外观如下(可以随意更改作者字段):

```toml
[package]
name = "playervault"
version = "0.1.0"
authors = ["A. Student"]
edition = "2018"

[lib]
crate-type = ["cdylib"]

[dependencies]
wapc-guest = "0.4.0"
wasmcloud-actor-keyvalue = { version = "0.2.1", features = ["guest"] }
wasmcloud-actor-core = { version = "0.2.2", features = ["guest"] }
wasmcloud-actor-http-server = { version = "0.1.1", features = ["guest"]}
serde_json = "1.0.59"
serde = { version = "1.0.125", features = ["derive"]}

[profile.release]
# Optimize for small code size
opt-level = "s"
lto = true
```

您可能还记得在课程的早期使用wapc-guest板条箱。wasmcloud-*板条箱是赋予我们的参与者通过主机与能力提供者通信的能力。我们将使用serde板条箱进行序列化和构造JSON有效负载。

接下来，让我们看看src/lib.rs中的actor的第一部分:

```rust
use serde::{Deserialize, Serialize};
use serde_json;
use wapc_guest::prelude::*;
use wasmcloud_actor_core as actor;
use wasmcloud_actor_http_server as http;
use wasmcloud_actor_keyvalue as kv;

#[actor::init]
fn init() {
      http::Handlers::register_handle_request(handle_http_request);
}
```

actor::init过程宏围绕我们在前一章讨论过的wapc_init函数创建一个自定义包装器。这个包装器还为我们提供了wasmCloud主机运行时所需的缺省健康检查处理程序。我们现在有了一些自定义消息处理程序注册函数，这些函数来自于actor接口箱，比如HTTP服务器。

在下一页继续。
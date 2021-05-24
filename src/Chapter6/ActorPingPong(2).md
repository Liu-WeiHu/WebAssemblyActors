# Actor Ping Pong (2)

看看“Ponger”项目的Makefile中的这一行:

```makefile
wash claims sign $(DEBUG)/ponger.wasm --name "ponger" -a "wasmcloud/examples/ponger" --ver $(VERSION) --rev 0
```

这为actor提供了一个名称“ponger”，但也为它提供了wasmcloud/examples/ponger的调用或调用别名。这意味着任何参与者都可以使用call_actor函数并提供“wamcloud/examples/ponger”，并且无论ponger参与者在星系的哪个位置，wasmCloud都会找到它并调用它。

我们也可以使用wash命令检查ponger的声明，验证它确实有一个调用别名:

<font color=Blue>wash claims inspect target/wasm32-unknown-unknown/debug/ponger_s.wasm</font>

```text
                     ponger - Module
 Account ABWB27PJPGIGRSYSZ55WGXZOWB3RDV3ESI2V2J5A4JUQLDP6NHM5YXAL
 Module MAS242Z2N4ZNGKGC2KGLVHLC4FBCDPF634TR563XBTNW3VR5RX25253X
 Expires                                                never
 Can Be Used                                            immediately
 Version                                                0.1.0 (0)
 Call Alias                           wasmcloud/examples/ponger
                          Capabilities

                          Tags
None
```

我们可以在这里看到，这个actor的调用别名是wasmcloud/examples/ponger。此处斜杠的使用仅适用于convention—wasmcloud参与者可以将最简单的字符串声明为别名。

现在我们只剩下一件事了，那就是编写pinger，或者actor，来响应一些消息，调用Pong actor。

```rust
use ping_interface as demo;
use wapc_guest as guest;

use serde::{Deserialize, Serialize};
use wasmcloud_actor_core as actor;
use wasmcloud_actor_http_server as http;

use guest::prelude::*;

#[actor::init]
fn init() {
     http::Handlers::register_handle_request(handle_request);
}

fn handle_request(_req: http::Request) ->
  HandlerResult<http::Response> {
    let p: demo::Pong = actor::call_actor(
                            "wasmcloud/examples/ponger",
                            "Ping",
                            &demo::Ping { value: 11 },
                            )?;
    Ok(http::Response::json(&p, 200, "OK"))
}
```
在下一页继续。
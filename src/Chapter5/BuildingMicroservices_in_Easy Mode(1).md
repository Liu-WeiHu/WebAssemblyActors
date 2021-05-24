# 以“简单模式”构建微服务(1)

让我们检查一个假设的场景。你被要求创建一个支持在线游戏的微服务。这个服务只负责管理玩家的金币数量。它必须支持原子存款或从玩家的金库提取金币，以及查询他们当前的余额。

您已经被告知，这需要以分布式方式工作，必须具有弹性的可伸缩性，数据最终必须是一致的，查询必须是快速的，并且您至少需要将此功能作为RESTful服务公开，但后端设计者希望保留公开不同接口的选项(比如通过处理来自NATS或RabbitMQ这样的代理的消息)。此时，您应该注意有多少需求是真正的“业务逻辑”，多少是[非功能性需求](https://en.wikipedia.org/wiki/Non-functional_requirement) (系统应该做什么，以及系统应该如何做)。

以“艰难的方式”构建服务时，我们将开始工作，很快就会淹没在样板文件中，更糟糕的是，各种客户端库和用于运行网络端点的库的紧密耦合依赖的泥沼中。通过多年的微服务开发，我们了解到，“[你拥有你的依赖](https://bambielli.com/posts/2017-05-06-the-amazon-way-pt-1/#2-take-ownership-of-results) ”这句话是生活的咒语。

我们的“硬方法”样板文件必须包括监听可配置端口的HTTP服务器的创建和配置。然后，我们必须配置从HTTP服务器端点到业务逻辑的路由规则。接下来，我们必须编写一组垫片或包装器，以使我们的业务逻辑能够访问一个最终一致的、读取优化的数据存储。当我们以这种方式构建时，我们的业务逻辑通常只代表了我们必须编写的不到10%的代码，而90%的代码是我们需要的“仪式”、样板和库依赖项。最糟糕的是，我们的代码现在与我们所选择的功能紧密耦合。我们使用的特定web服务器被编译成二进制文件，就像数据库/缓存客户端一样。如果我们需要对这些代码进行更改，或者在这些代码中发现关键漏洞，我们将被迫修改代码、重新构建和重新部署。

wasmCloud的主张是，以WebAssembly为核心，有一个更好的方法。

我们知道如何以老式的方式构建这个解决方案，所以让我们看看作为wasmCloud应用程序它是什么样子的。摆脱了紧耦合能力的限制，我们可以通过编写一些伪代码来设计我们的解决方案:

```text
handle QueryVault ->
  get player ID from message
  load balance
  return balance
handle Deposit ->
  get player ID and amount from message
  atomic increment balance
  return success
handle Withdraw ->
  get player ID and amount from message
  validate amount
  atomic decrement balance
  return success
```

这段代码缺少的是实现细节。我们并不是指特定的实现，比如Redis或Cassandra用于数据存储，Rocket或Actix-Web用于HTTP服务器库。如果我们能从这个伪代码到一个编译后的WebAssembly actor做一个小小的跳跃，不是很好吗?

在下一页继续。
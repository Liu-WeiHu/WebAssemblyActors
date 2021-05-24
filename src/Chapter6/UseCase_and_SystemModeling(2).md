# 用例和系统建模(2)

假设税收参与者知道如何理解ProductItems类型的结构，我们可以通过这个简单的调用轻松地检索购物车当前内容的税收数据:

```rust
let tax_data: TaxInfo = call_actor(“mycorp/taxes”,
                          “CalculateTax”,
                          current_cart.items);
```

这种安排的微妙之处在于，我们为参与者提供了一个人类可读的标识符，我们希望调用的操作的名称，以及要发送给该参与者的一些可序列化的有效负载。请记住，在某些语言中，调用calculate_tax函数实际上只不过是将“calculate tax”消息发送给接收方。

我们的代码在任何时候都不知道或关心税收参与者的实例数量、这些实例在哪里、内部网络机制如何工作，甚至不关心税收系统如何处理并发请求。我们只是声明了调用业务逻辑的意图。

在tax actor内部，我们可以使用如下简单代码声明一个处理程序:

```rust
#[actor::init]
fn init() {
    taxes::Handlers::register_calculate_tax(calculate_handler);
}
```

然后，calculate handler函数有一个常规的函数签名，就像所有其他actor消息处理器一样:

```rust
fn calculate_tax(msg: ProductLineItems) -> HandlerResult<TaxData> {
    // do work, use capabilities, etc
    // ...
    Ok(res)
}
```
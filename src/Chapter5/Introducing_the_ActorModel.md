# 什么是Actor模型?

在计算机科学中，[参与者模型](https://en.wikipedia.org/wiki/Actor_model) 是将参与者视为并发计算的通用原语的模型。这个短语是维基百科的定义，但是“并行计算的通用原语”是什么意思呢?这意味着在构建并发系统时，您可以使用的最小的计算构建块是参与者——如果您愿意的话，可以将其称为数字乐高积木。Wikipedia还列出了参与者的以下功能，所有这些功能都是在响应接收到的消息时完成的:
- 当地做决定
- 创建角色
- 发送消息
- 确定如何响应下一条消息
- 修改内部状态
- 仅通过消息传递与所有其他参与者进行通信

使用actor模型进行开发的最重要影响之一是，当开发人员创建actor时，他们不知道并发性的方法，也不依赖于这种方法。他们在actor中编写的一切都是简单、直接、单线程代码。所有并发和同步都是通过消息传递和提供角色模型实现的框架在外部处理的。通过将开发人员从必须显式地管理线程和并发性中解放出来，他们可以专注于支持他们正在构建的特性所需的业务逻辑，从理论上讲，构建的代码不容易出错。

许多语言以非常不同的方式处理并发性。我们大多数人都熟悉使用一些通常被称为[有颜色的函数](https://journal.stuffwithstuff.com/2015/02/01/what-color-is-your-function/) 的语言。应用于并发系统的带有颜色的函数意味着该语言要求开发人员在设计时指明函数是异步的还是同步的。函数的同步或异步特性永久地与函数的调用签名绑定在一起。正如链接的博客文章所指出的，这本质上创建了一种“(a)同步反压力”，迫使函数的所有消费者具有相同的色调(并发类型)。

在经历过许多同步耦合函数的实现之后，我们可以保证这种体验并不理想，阻力很大，而且经常容易出错。如果您有兴趣了解这个特定的问题目前如何影响Rust社区，您可以从Rust异步书籍的[这一节](https://rust-lang.github.io/async-book/01_getting_started/03_state_of_async_rust.html) 开始。最近还成立了一个[工作组](https://rust-lang.github.io/wg-async-foundations/) ，致力于改进《Rust》中的异步体验。

角色模型只是使用(一个)同步元数据装饰函数的众多替代方案之一。在参与者模型中，并发性在参与者外部处理，在参与者内部，所有处理消息的代码都同步执行。actor模型有相当多的具体实现，从[Erlang的OTP](https://erlang.org/doc/) 这样久经磨练的框架，到[Pony](https://www.ponylang.io/) 这样的新语言，再到流行的基于jvm的[Akka框架](https://akka.io/) 。


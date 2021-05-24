# 在wasmCloud Shell中运行参与者(3)

您将看到大量的日志输出，我们现在还不需要担心这些输出。当输出在几秒钟后稳定下来，并且你不再看到新消息时，我们可以测试玩家跳马服务。

在一个新的终端中，发出以下命令来查询当前保险库余额:

<font color=Blue>curl localhost:8080/vault</font>

```text
{"gold":0}
```

现在我们来存放一些金子:

<font color=Blue>curl -X PUT http://localhost:8080/vault -d '{"gold": 100}'</font>

如果工作正常，我们不会看到任何输出，因为服务只返回一个200/OK。 现在我们可以再次查询金库来确认我们现在确实有100枚金币  

<font color=Blue>curl localhost:8080/vault</font>

```text
{"gold":100}
```

现在让我们来测试一下我们服务的最后一个功能:

<font color=Blue>curl -X DELETE http://localhost:8080/vault -d '{"gold": 12}'

curl localhost:8080/vault
</font>

```text
{"gold":88}
```

从表面上看，我们似乎已经经历了很多，仅仅是为了创建这项服务。然而，让我们考虑一些事情。首先，我们知道wasmCloud保证我们的参与者只能访问我们授予的权限。我们还知道，因为我们正在使用提供者合约抽象，所以我们可以用Cassandra或memcache或自定义的内存中提供者来替换Redis键值存储，而无需重构、重新编译或重新部署我们的参与者。类似地，我们可以在主机运行时仍在运行时实时升级actor，而不会丢失任何一条消息。如果这还不够强大(我们将在稍后的课程中看到)，我们还知道，当我们在[网格](https://wasmcloud.dev/reference/lattice/) 中部署参与者时，它们可以跨不同的基础设施与其他参与者和功能提供者无缝地通信。最后，我们的代码现在几乎没有样板，很容易构建和维护。
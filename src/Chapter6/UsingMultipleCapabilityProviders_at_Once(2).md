# 同时使用多个能力提供者(2)

由结构体ChannelMessage描述的入站消息通过管道运行，在管道中对其进行验证，然后发布到消息代理，然后发送到事件流进行持久化。在传统microservice甚至λ,出版代理消息的行为或发射事件流是将负载与大量的样板代码,包括通常相当于一座山的代码需要建立一个收集我们的代理(NATS,卡夫卡,RabbitMQ等等)以及我们的事件流(Kafka，事件存储等)。

而且，这段代码确实与所有这些东西紧密耦合。如果我们想把我们的消息代理从RabbitMQ切换到Kafka，然后切换到一些云提供的服务(比如Amazon Kinesis)，那么我们必须重构、重写、重新测试、重建和重新部署我们的服务。

因为这些样板文件或紧密耦合都不存在于wasmCloud中编写的角色中，所以我们没有这些负担。下面是向代理发布消息的代码:

```rust
fn publish_broker_message(
     target: &MessageTarget,
     message: &messages::ChannelMessage,
) -> HandlerResult<()> {
     info!("Publishing message to back-end");
     let topic = match target {
        MessageTarget::User(s) => format!("{}.user.{}",
                                  BACKEND_SUBJECT_PREFIX, s),
        MessageTarget::Room(s) => format!("{}.room.{}",
                                  BACKEND_SUBJECT_PREFIX, s),
        _ => "".to_string(),
     };
     let ce = CloudEvent::new_json(
         &message.message_id,
         EVENT_TYPE_MESSAGE_PUBLISHED,
         EVENT_SOURCE,
         message.created_on,
         &message,
     );

     broker::host(LINK_BACKEND)
       .publish(topic, "".to_string(),
                 serde_json::to_vec(&ce)?)?;
     Ok(())
}
```

注意这里缺少什么:连接字符串、配置参数，当然还有代理的供应商或提供者。这段代码只关心发布了什么，而不是如何发布。要获得这个例子的完整源代码，请查看[wasmcloud-chat演示](https://github.com/wasmCloud/examples/tree/main/wasmcloud-chat) 。
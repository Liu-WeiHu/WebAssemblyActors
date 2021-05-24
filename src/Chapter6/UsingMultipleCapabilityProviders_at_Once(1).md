# 同时使用多个能力提供者(1)

当您看到响应传入HTTP服务器能力提供者消息的参与者，然后在响应中调用另一个参与者或另一个能力提供者时，您已经看到了一点这方面的内容。在结束本章之前，我们想向您展示一些真实世界的代码，这些代码用于一个功能完整的应用程序，有助于说明角色组合的强大功能和多个提供者的使用。

下面的代码片段取自分布式聊天应用程序中的消息参与者，该应用程序支持多个实时的连通性通道，包括NATS、web服务器，甚至telnet。在我们提供其他上下文之前，先看一看下面的代码:

```rust
// Processing a message:
// 1 - validate the message
// 2 - publish it to the appropriate message broker subject
// 3 - emit event to event stream
// 4 - ACK
fn process_message(message: ChannelMessage) ->
    HandlerResult<ProcessAck> {
  info!(
     "Processing inbound channel message from '{}'",
     message.origin_channel
     );
   let target = MessageTarget::from(&message);
   if let MessageTarget::Unknown(message) = target {
      let msg =
        format!("Unable to select target for message: {}",
               message);
      error!("{}", msg);
      return Err(msg.into());
   }

   let ack = validate_message(&message)
             .and_then(|_| publish_broker_message(&target, &message))
             .and_then(|_| emit_stream_event(&target, &message))
             .map_or_else(
             |e| {
               ProcessAck::fail(
               &message.message_id,
               &format!(
               "Failed to publish and emit to event stream: {}", e),
              )
             },
             |_v| ProcessAck::success(&message.message_id),
   );
   info!("Acking: {:?}", ack);
   Ok(ack)
}
```

在下一页继续。
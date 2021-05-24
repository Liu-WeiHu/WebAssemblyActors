# Actor Ping Pong (3)

最后，我们将为此需要一个wasmCloud主机运行时。它将需要以下内容:
- The Ponger actor
- The Pinger actor
- HTTP服务器功能提供者
- 一个链接定义，它配置了Pinger参与者侦听的端口

值得庆幸的是,我们学习了如何创建一个清单文件在前面的章节中,我们来看看一种看起来像(模块的公钥将比在不同的机器上,所以使用适当清洗检查模块和导出环境变量):

```text
---
actors:
  - ./pinger/target/wasm32-unknown-unknown/debug/pinger_s.wasm
  - ./ponger/target/wasm32-unknown-unknown/debug/ponger_s.wasm
capabilities:
     - image_ref: wasmcloud.azurecr.io/httpserver:0.12.1
links:
  # the ponger needs no link definition because it doesn't
  # use the HTTP server provider
  - actor: ${PINGER_ACTOR}
     contract_id: "wasmcloud:httpserver"
     provider_id: "VAG3QITQQ2ODAOWB5TTQSDJ53XK3SHBEIFNK4AYJ5RKAX2UNSCAPHA5M"
     values:
     PORT: 8080
```

请记住，功能提供者有一个固定的公钥(因为它总是由官方的wasmCloud私钥签名)，所以您不需要在清单中更改它。一旦你发现了pinger模块的公钥，你就可以像这样设置环境变量(Windows语法会有所不同):

<font color=Blue>export PINGER_ACTOR=MB2ZOJXL67TBHKRPHX6EE3PUAX7QASN3SDX4ALNPO272HB42IBYUA6BV</font>

现在我们可以用manifest启动wasmCloud主机二进制文件:

<font color=Blue>wasmcloud -m manifest.yaml</font>

```text
[2021-04-18T14:55:10Z INFO ] Message Bus started
(lots and lots of log output)
…
```

如果一切都解决了，我们可以卷曲本地pinger actor，它将向ponger发送值11,ponger将反过来向它添加42并响应。我们应该看到53的值，就像这样:

<font color=Blue>curl localhost:8080</font>

```text
{"value":53}
```

对于所有示例代码，您可以在https://github.com/wasmCloud/examples存储库的参与者到参与者文件夹中找到它。您应该能够在pinger和ponger参与者上运行make，导出PINGER_ACTOR环境变量，然后通过清单文件运行示例。
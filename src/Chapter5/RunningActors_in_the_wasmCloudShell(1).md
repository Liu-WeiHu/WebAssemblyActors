# 在wasmCloud Shell中运行参与者(1)

现在我们已经得到了一个已编译成.wasm文件的actor，让我们看看如何在wasmCloud主机运行时中运行这个actor。在我们运行一个actor之前，我们需要签署它。wasmCloud是一个安全的、默认拒绝的运行时。当您尝试加载一个参与者时，wasmCloud将检查一个嵌入式的、签名的JSON Web令牌(JWT)，该令牌描述了参与者可以做什么。在我们的示例中，我们希望授予参与者使用键值存储提供程序和HTTP服务器提供程序的能力。为此，我们可以在终端提示符中输入以下命令:

<font color=Blue>wash claims sign target/wasm32-unknown-unknown/debug/playervault.wasm -k -q --name "Player Vault"</font>

```text
No keypair found in "/home/kevin/.wash/keys/playervault_module.nk".
We will generate one for you and place it there.
If you'd like to use alternative keys, you can supply them as a flag.

Successfully signed target/wasm32-unknown-unknown/debug/playervault_s.wasm with capabilities: wasmcloud:keyvalue,wasmcloud:httpserver
```

输出表明我们已经生成了一个用于对参与者签名的新私钥。为了验证我们的actor是否可以被wasmCloud运行时使用，我们可以使用wash claims inspect:

<font color=Blue>wash claims inspect target/wasm32-unknown-unknown/debug/playervault_s.wasm</font>

```text
                          Player Vault - Module
Account        ABWB27PJPGIGRSYSZ55WGXZOWB3RDV3ESI2V2J5A4JUQLDP6NHM5YXAL
Module         MAHUHHQNILLDRPX5IRNOVUH2U2SCTSJ636E42G4TCIBKAQFOBUSS74TW
Expires                                                               never
Can Be Used                                                     immediately
Version                                                         None (0)
Call Alias                                                     (Not set)
                               Capabilities
K/V Store
HTTP Server
                                    Tags
None
```

在您的机器上，帐户公钥和模块公钥将是不同的，因为它们是从签名期间生成的私钥派生出来的。

有几种方法可以测试这个演员。第一种是使用wash repl，你可以通过在终端窗口中输入以下命令来启动它:

<font color=Blue>wash up</font>

在下一页继续。
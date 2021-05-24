# 运行中的waPC(3)

WapcHost结构的构造函数接受一个实现WebAssemblyEngineProvider特征的装箱对象和一个用于处理来自客户的调用请求的回调的闭包或函数。

load_file函数只是从我们的WebAssembly模块检索原始字节。handle_callback函数如下所示:

```rust
fn handle_callback(
  _binding: &str,
  _namespace: &str,
  operation: &str,
  _payload: &[u8],
) -> Result<Vec<u8>, Box<dyn Error + Send + Sync>> {
  if operation == "GetGreeting" {
    Ok(b"Ahoy There".to_vec())
  } else {
    Err("Unsupported host call!".into())
  }
}
```

关于我们的回调处理程序的一个有趣的事情是，如果客户尝试调用不支持的操作，我们可以返回本地Rust错误。

最后，如果我们运行hellohost项目，应该会看到类似如下的输出:

<font color=Blue>cargo run</font>

```text
Compiling hellohost v0.1.0 (/home/kevin/Code/edX/wasm_actors/wapc/hellohost)
      Finished dev [unoptimized + debuginfo] target(s) in 5.52s
      Running `target/debug/hellohost`
Ahoy There, Alice!
```

此时，您应该已经获得了在自定义主机运行时和可移植WebAssembly模块之间构建健壮的双向通信所需的信息和工具。使用不透明二进制有效负载支持任意操作调用的能力，无论主机或客户机使用什么语言(或内存管理策略)。
# 创建主机(1)

创建主机首先要创建一个新的Rust二进制项目。我们可以使用下面的命令来创建主机(从创建导入器的父目录执行):

<font color=Blue>cargo new host</font>

Rust的默认箱子类型是一个独立的二进制文件，所以这就是我们想要的。让我们看一下主机的代码(wasm3作为一个依赖项添加到我们的项目中):

```rust
use std::io::Read;
use std::{error::Error, fs::File};

use wasm3::{CallContext, Environment};
use wasm3::Module;

fn main() -> Result<(), Box<dyn Error>> {

     let env = Environment::new()?;
     let rt = env.create_runtime(1024 * 60)?;

     let bytes = {
       let mut f =
      File::open(
 "../importer/target/wasm32-unknown-unknown/release/importer.wasm")?;

       let mut bytes = vec![];
       f.read_to_end(&mut bytes)?;
       bytes
     };

     let module = Module::parse(&env, &bytes)?;
     let mut module = rt.load_module(module)?;

     if let Err(_e) = module.link_closure(
        "utilities",
        "random",
        move |_ctx: &CallContext, ()| -> i32 {
           use rand::Rng;
           let mut rng = rand::thread_rng();

           rng.gen_range(0..=100)
         }) {
         return Err("Failed to link closure".into());
     }

     let addto = module.find_function::i32, i32("addto")?;

     println!("Result: {}", addto.call(10)?);

     Ok(())
}
```
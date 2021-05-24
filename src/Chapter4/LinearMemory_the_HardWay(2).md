# 艰难的线性记忆(2)

如果您开始考虑内存分配器是如何工作的，以及它们可能给这个问题增加的复杂性，那么您就走上了正确的道路。对于我们正在构建的示例，我们不需要分配器，因为我们只需要管理一个缓冲区。

当调用set_name函数时，guest模块将缓冲区中的名称替换为主机可以使用的问候语。这里还值得注意的是，我们没有进行任何长度检查——如果连接到“Hello”的名称超过了内存缓冲区的大小，此代码将崩溃。

现在让我们看一下主机代码。除了使用不安全的指针算术从模块的内存数组读取和写入的代码外，大多数情况看起来应该很熟悉:

```rust
use std::io::Read;
use std::{error::Error, fs::File};

use wasm3::{Environment, Runtime};
use wasm3::Module;

fn main() -> Result<(), Box<dyn Error>> {
  let env = Environment::new()?;
  let rt = env.create_runtime(1024 * 60)?;
  let mut f =
    File::open(
"../memoryguest/target/wasm32-unknown-unknown/debug/memoryguest.wasm"
      )?;
  let mut bytes = vec![];
  f.read_to_end(&mut bytes)?;
  let module = Module::parse(&env, &bytes)?;
  let module = rt.load_module(module)?;

  // Get the pointer (index) of the buffer
  let getptr = module.find_function::<(), i32>("get_buffer_ptr")?;
  let ptr = getptr.call()?;
  println!("{}", ptr);

  // Write the name to memory
  let name = b"Tester McTesto";
  write_bytes_to_memory(&rt, ptr, name);

  // tell the guest we set the name
  let set_name = module.find_function::<i32, i32>("set_name")?;
  let new_len = set_name.call(name.len() as i32)?;
  println!("New string length: {}", new_len);

  let new_bytes = get_vec_from_memory(&rt, ptr, new_len);
  let new_string = std::str::from_utf8(&new_bytes)?;
  println!("Response: {}", new_string);

  Ok(())
}

fn get_vec_from_memory(rt: &Runtime, ptr: i32, len: i32) -> Vec<u8> {let data = unsafe { &*rt.memory() };

  data[ptr as usize..][..len as usize].to_vec()
}

fn write_bytes_to_memory(rt: &Runtime, ptr: i32, slice: &[u8]) {
  unsafe {
   (&mut *rt.memory_mut())[ptr as usize..][..slice.len()].copy_from_slice(slice);
  };
}
```

在下一页继续。
# 创建一个Wasm3主机(2)

现在修改src/main.Rs文件，如下所示:

```rust
use std::io::Read;
use std::{error::Error, fs::File};

use wasm3::Environment;
use wasm3::Module;

fn main() -> Result<(), Box<dyn Error>> {
     let env = Environment::new()?;
     let rt = env.create_runtime(1024 * 60)?;
     let mut f = File::open("../calc.wasm")?;
     let mut bytes = vec![];
     f.read_to_end(&mut bytes)?;
     let module = Module::parse(&env, bytes)?;
     let module = rt.load_module(module)?;

     let add = module.find_function::<(i32, i32), i32>("add")?;
     let sub = module.find_function::<(i32, i32), i32>("sub")?;
     let mul = module.find_function::<(i32, i32), i32>("mul")?;
     let div = module.find_function::<(i32, i32), i32>("div")?;

     println!("Adding 3 and 2: {}", add.call(3, 2)?);
     println!("Subtract 10 and 2: {}", sub.call(10, 2)?);
     println!("Multiply 3 and 2: {}", mul.call(3,2)?);
     println!("Divide 10 and 2: {}", div.call(10,2)?);

     Ok(())
}
```

在加载和解析WebAssembly模块的初始过程中，几乎每个方面都可能失败，因此我们简化了主函数，允许它返回Result类型。我们做的第一件事是创建一个新的环境。我们不会详细讨论这个问题，但是当您使用额外的功能(如WASI)时，这个步骤将变得更加重要。

接下来，我们读取WebAssembly文件的原始字节(如果计算器模块存在于环境的其他地方，则可能需要更改路径)，然后解析这些字节。假设解析器没有报告任何验证错误，然后我们创建一个模块的运行时实例。

接下来，我们使用find_function函数定位具有特定名称的导出，并使用Rust泛型的强大功能为它们分配方法签名。注意，如果按名称指定一个函数，而它的签名不能强制匹配您所指定的，这将产生一个错误。我们在上一个示例中编写的JavaScript代码仍然在做类似的工作，但是WebAssembly API向您隐藏了其中的一些复杂性。在这种情况下，JavaScript的动态类型还向开发人员隐藏了相当多的复杂性。

最后，有了所有的样板文件，我们可以自由地调用所有计算器函数。你可以在project目录下运行这个例子:

<font color=Blue>cargo run</font>

```text
     Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/rust-host`
Adding 3 and 2: 5
Subtract 10 and 2: 8
Multiply 3 and 2: 6
Divide 10 and 2: 5
```
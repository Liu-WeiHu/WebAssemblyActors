# 构建一个WebAssembly单词计数器(1)

为了比较独立的WebAssembly模块和WASI模块，让我们构建一个单词计数应用程序。首先应该注意的是，我们说的是应用程序，而不是库或模块。有了wasmtime这样的运行时，就可以“运行”WASI模块。

首先，在你的电脑上找到一些空的空间，并创建一个新的Rust二进制项目，如下所示:

<font color=Blue>cargo new --bin wordcounter</font>

这个应用程序将从命令行上传递的第一个参数所指示的文件中读取文本。然后它将处理该文件，计算遇到的每个单词的数量，然后将这些计数打印到stdout—所有这些在独立目标中是不可能的。

从src/main中删除样板文件。Rs，用以下代码替换:

```rust
use std::collections::BTreeMap;
use std::env;
use std::fs;
use std::io::BufRead;

fn process(input_fname: &str) ->
           Result<BTreeMap<String, usize>, String> {

   let input_file = fs::File::open(input_fname)
        .map_err(|err| format!("error opening input {}: {}",
                               input_fname, err))?;

   let mut counts: BTreeMap<String, usize> = BTreeMap::new();

   for line in std::io::BufReader::new(input_file).lines() {
      let line = line.expect("error parsing line!");

     for word in line.split_ascii_whitespace().map(str::to_lowercase)
     {
        let word = word.trim_matches(|c: char| !c.is_alphanumeric());
        *counts.entry(word.into()).or_insert(0) += 1;
     }
   }

   Ok(counts)
}

fn main() {
   let args: Vec<String> = env::args().collect();
   let program = args[0].clone();

   if args.len() < 2 {
      eprintln!("usage: {} <input_file>", program);
      return;
   }

   match process(&args[1]) {
      Ok(counts) => render_counts(&counts),
      Err(err) => eprintln!("{}", err),
   }
}

fn render_counts(counts: &BTreeMap<String, usize>) {
   for (word, count) in counts.iter() {
     println!("{} {}", word, count);
   }
}
```

如果您是Rust的新手，那么这个代码一开始可能看起来有点奇怪。经过几次检查，它应该开始有意义，除了几行重要的行。进程函数中的代码将输入文件分成几行，然后按空格将每行分成单词。然后，它会去掉这个单词中除字母数字以外的所有内容(例如，标点符号、带空格的括号等)。

接下来，我们看到这一行:

<font color=Blue>*counts.entry(word.into()).or_insert(0) += 1;</font>

这将使用单词作为键在计数映射中检索或创建一个条目(值为0)，并将1添加到发生计数中。它有点简洁，但仍然相当有表现力。对于好奇的人来说，Entry API是标准库中一个非常受称赞的特性。

在下一页继续。
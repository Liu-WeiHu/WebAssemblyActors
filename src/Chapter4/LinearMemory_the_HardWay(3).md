# 艰难的线性记忆(3)

下面是主机正在执行的步骤的概要:
1. 获取要用作线性内存数组索引偏移量的缓冲区指针。
2. 将原始字节(碰巧是UTF8编码的)写入数组。
3. 调用set_name来告诉guest模块名称的长度并检索问候语的长度。
4. 使用指针和返回的新长度从模块内存中检索更新后的字符串。

这段代码中有两个额外的print语句，帮助说明这里发生的事情的更多内部工作原理。当我们从**memory/memoryhost**文件夹运行这个例子时，我们会看到以下输出:

<font color=Blue>cargo run</font>

```text
Finished dev [unoptimized + debuginfo] target(s) in 0.02s
     Running `target/debug/memoryhost`
1053104
New string length: 21
Response: Hello, Tester McTesto
```

内存地址(原始指针值)在您的机器上应该不同于这个输出，但其他一切应该是相同的。

正如您可以想象的那样，这个解决方案相当脆弱，当然不适用于实际的用例。我们需要处理诸如缓冲区的收缩和增长、错误处理等等。
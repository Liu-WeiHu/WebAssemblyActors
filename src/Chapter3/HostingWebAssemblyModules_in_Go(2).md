# 在Go中托管WebAssembly模块(1)

这里的流动与Rust主机非常相似。首先，我们从磁盘加载WebAssembly模块，这样我们就可以将它视为一个字节数组(在本例中是切片)。接下来，我们将从WebAssembly字节创建一个引擎、一个存储和一个模块。此时，模块只是一个模块，而不是模块的实例。在接下来的两行中，我们创建了一个空的导入对象(稍后我们将讨论这些)，并使用该对象和模块来创建一个模块实例。

有了一个实例，我们就可以查找add函数。注意，这里是Go和Rust代码的不同之处。Go将允许我们按名称加载函数，并能够推断签名并公开可变函数，而我们必须使用一些泛型诡计使之在Rust中工作。最后，我们可以调用我们检索到的函数并得到一个结果。

在终端提示符下，从go-host目录运行以下命令:

<font color=Blue>go run gohost.go</font>

```text
10
```

现在你应该能够添加更多的行来创建可用的乘法、减法和除法函数:

```go
add, _ := instance.Exports.GetFunction("add")
mul, _ := instance.Exports.GetFunction("mul")
div, _ := instance.Exports.GetFunction("div")
sub, _ := instance.Exports.GetFunction("sub")

result, _ := add(2, 8)
fmt.Println(result)
result, _ = mul(2, 5)
fmt.Println(result)
result, _ = div(10, 2)
fmt.Println(result)
result, _ = sub(10, 2)
fmt.Println(result)
```

现在，当你运行Go模块时，你应该会看到以下输出:

```text
10
10
5
8
```
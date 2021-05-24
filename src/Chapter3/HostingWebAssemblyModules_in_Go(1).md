# 在Go中托管WebAssembly模块(1)

尽管我们没有做太多的事情，但我们提供了一个Go文件，它承载了一个WebAssembly模块，让你运行前面章节中的井字棋实验室。

在Go中托管WebAssembly模块与在Rust中托管WebAssembly模块大致相同:你需要找到一个可以JIT编译或解释WebAssembly模块的库，然后编写一些使用该库的代码。

如果Go不是你喜欢的语言，别担心。这些代码非常基础，即使您不经常使用Go，也应该很容易理解。为了增加多样性，我们将使用一个叫wasmer的图书馆。为了便于比较，我们仍然要使用前一章开发的计算器模块。

首先，要确保在您的工作站上有wasmer可用，您可以发出以下Go命令来安装它(如果您能够运行前面章节中的实验室，您应该已经安装了它):

<font color=Blue>export CGO_ENABLED=1; export CC=gcc;

go get github.com/wasmerio/wasmer-go/wasmer
</font>

如果您有任何问题，请检查以确保您正在运行Go 1.14或更高版本。此外，在编写本课程材料时，Wasmer还不支持在Windows上运行，因此，如果您有一台Windows机器，并且希望继续学习，那么您需要使用一台运行Linux的虚拟机，或者尝试使用Windows子系统for Linux (WSL)。

创建一个名为go-host的新目录，并在其中放置一个名为gohost.go的文件。设置文件的内容如下:

```go
package main

import (
    "fmt"
    "io/ioutil"

    wasm "github.com/wasmerio/wasmer-go/wasmer"
)

func main() {
    wasmBytes, _ := ioutil.ReadFile("../calc.wasm")

    engine := wasm.NewEngine()
    store := wasm.NewStore(engine)

    module, _ := wasm.NewModule(store, wasmBytes)

    importObject := wasm.NewImportObject()
    instance, _ := wasm.NewInstance(module, importObject)

    add, _ := instance.Exports.GetFunction("add")

    result, _ := add(2, 8)

    fmt.Println(result)
}
```

在下一页继续。
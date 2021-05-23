# WebAssembly特点:小

WebAssembly可以产生非常小的工件。今天，你可以使用像C、c++、Rust、Zig、Go、AssemblyScript和WebAssembly文本格式(wat)这样的语言来构建wasm模块。根据构建的内容和使用的工具链，您可以构建小于1MB的功能完整的模块。wasm的部署规模比我们今天在容器或无服务器“包”的世界中构建的大多数要小得多，这将对我们如何思考和规划分布式应用产生巨大的影响。
# 调用和满足导入

到目前为止，在这一章中，我们已经对导入的概念进行了隐喻性的讨论。在开始使用导入之前，我们希望您了解托管一个简单的独立WebAssembly模块是什么感觉。

正如我们在本章开始时所讨论的，导入可以被认为只是“来自另一个模块的函数”。无论该模块是来自WebAssembly模块的真实模块，还是宿主使用本机函数支持的模块，执行导入的WebAssembly模块仍然是未知的。

在大多数以WebAssembly为目标的语言中，调用一个导入的函数需要声明一个外部函数，通常用Rust和类c语言中的extern关键字表示。当支持WebAssembly的编译器看到这些extern时，编译器会将它们转换为WebAssembly导入，而不是假设它们将满足运行时库的要求。

在这个例子中，我们将使用Rust。它有点做作，但它应该能让您了解模块、导入和主机运行时之间的相互作用。在本例中，我们将创建一个WebAssembly模块，该模块从名为utilities的模块中导入一个名为random的函数。WebAssembly模块无法访问操作系统或系统时钟，甚至无法生成随机数。因此，要创建一个向随机数添加值的函数，我们需要从能够生成该值的主机导入随机数生成器。
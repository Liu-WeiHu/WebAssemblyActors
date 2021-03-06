# WebAssembly是什么?

在课程介绍中，我们做出了一个相当大胆的断言，WebAssembly是分布式计算的未来。这是一个非常崇高的目标，如果没有一些证据支持，这并不意味着什么。在本课程中，您将看到大量这样的证据，但首先让我们深入了解WebAssembly本身的一些细节。

我们可以从消除WebAssembly只能在web上工作，或者它在某种程度上只为在web浏览器的范围内运行而设计的神话开始。[Jay Phelps](https://www.infoq.com/presentations/webassembly-intro/) 创造了一个现在在WebAssembly社区中经常使用的短语:“WebAssembly既不是web也不是assembly”。

[WebAssembly规范](https://webassembly.github.io/spec/) 努力维护这些标准不仅适用于浏览器主机，还适用于任何其他兼容的主机运行时(规范所称的嵌入器)。在本课程的大部分时间里，我们将重点关注从云到边缘的任何地方都能运行的嵌入式程序，而在浏览器上只花很少的时间。

WebAssembly二进制格式是一种虚拟机格式。顾名思义，虚拟机运行时负责在虚拟机内部执行操作。在VirtualBox和VMware这样的软件中，这些虚拟机负责处理CPU级别的操作——它们假装是您安装操作系统的计算机。

WebAssembly虚拟机不会假装是CPU或操作系统。WebAssembly是一个基于堆栈的虚拟机。这意味着运行时负责处理堆栈级别的操作。WebAssembly操作代码(通常缩写为opcode)不能看到或操作堆栈和其他一些基本组件以外的任何东西。这是一个宿主运行时的工作，就像浏览器或其他嵌入式程序解释这些WebAssembly(在本课程中我们经常将其简化为“wasm”)指令，并维护堆栈代表他们。由于WebAssembly的指令集相对较小，并具有许多其他关键特性，因此它展示了在云中或在边缘运行的工作负载所高度追求的相同类型的特征。让我们更详细地讨论一下。
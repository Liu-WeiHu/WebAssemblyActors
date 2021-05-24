# Rust生存指南:易变性和最终对Borrow Checker的厌恶

在编译时跟踪Rust中每个变量的潜在生命周期。如果编译器注意到一个给定的变量可能不能为它的所有消费者提供一个真实的值(例如，不能引用丢失的或无效的内存块)，那么你的构建就会失败。

即使您从概念上知道所讨论的变量将存在足够长的时间，Rust编译器仍然需要您证明它。有时这需要使用明确的生命周期说明符(远远超出了这个类的范围)，制作克隆，使用特殊的引用类型，如Rc或Arc，以及许多其他最终成为Rust开发人员的第二天性的技术。

我们可以提供的最后一个建议是，Rust中的所有值在默认情况下都是不可变的。这样做有一个很好的理由:您可以有任意多的不可变引用，但您只能有一个可变引用。该规则跨块、函数、模块、线程边界等。您甚至可以分配对可变数据的不可变引用，只要您的代码在不可变引用的生命周期内不改变数据(现在开始明白为什么生命周期跟踪对Rust如此重要了吧?)

即使您知道代码中的所有有效路径都不会同时产生相同数据的多个变化，编译器也会要求您用正确的范围和生命周期管理来证明这一点。在Rust的初级阶段，这常常让人感觉做了很多工作，而其他语言做起来更容易，代码行更少。

这一切都不是为了吓唬你。相反，它希望能让您了解为什么当您开始侵占某些新型的Rust代码时，您开始收到来自borrow checker的越来越多的错误和投诉。

最后，Rust编译器的错误信息是我们所遇到的所有语言中最友好的。学习的最强大的来源之一实际上可能是您在构建周期中看到的错误消息。

好消息是，当我们将Rust编译到WebAssembly时，我们只使用了该语言的一个有限子集，因此我们不太可能遇到它的一些更复杂的用例。
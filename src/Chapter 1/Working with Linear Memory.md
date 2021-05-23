# 使用线性存储器

到目前为止，我们遇到的所有指令都是处理堆栈的。WebAssembly是一个堆栈机器，指令获取它们的参数并通过与堆栈交互返回它们的值。尽管堆栈快速而高效，但如果不能使用堆，我们就无法创建任何有用或复杂的模块。

在WebAssembly中，我们传统的堆概念是由所谓的线性内存来满足的。这听起来很简单。线性存储器是线性排列的单个、连续的存储器块。换句话说，它是一个可索引的字节数组。

WebAssembly模块中的线性内存是以页面为单位分配的，其中每个页面是64KiB的块(有趣的事实是:kilobyte是1000,kibibyte是1024)。有四个与处理线性存储器有关的主要指令:
- **memory.size** - Returns the size, in pages, of the module’s current linear memory limit
- **memory.grow** - Requests that memory be expanded by the given number of pages
- **(type).store** - Stores a value of the given data type (e.g. i32) at the given memory location
- **(type).load** - Retrieves a value of the given data type from the given memory location

还有一个高级语言提供给我们可以忽略的细节，就是WebAssembly以小端方式在内存中存储数字。系统的“字节顺序”指的是字节顺序。Little-endian是指按照从最小到最高的顺序存储字节，大端则相反。
# 艰难的线性记忆(1)

关于低层次细节教学的理念往往各不相同。并不是所有人都想知道内燃机是如何使汽车通过一系列精确定时的微爆炸的。有时候，人们只想坐上他们的车就走。

当涉及到线性内存的管理时，我们认为了解它在基本层面上是如何工作的，对于能够欣赏官方和社区工具将为您完成的工作是至关重要的。bindgen工具非常出色，它为您做了很多繁忙的工作。然而，通过“艰难的方式”处理线性内存将帮助您在更高抽象级别上设计和使用基于WebAssembly基础的复杂解决方案。

在下一组代码示例中，我们将演示主机和客户机之间完整的往返数据交换过程。这个简单的交流需要注意大量的细节。

我们想要构建的交换本质上是线性内存管理的基本“hello world”。以下步骤描述了交换过程:
1. 主机设置一个名称并将其提供给客户机
2. 客人检索名字并从中产生问候，例如“Hello, Alice”
3. 然后，主机从客人那里检索新修改的问候语。

这种类型的交换，看起来是一个简单的函数调用，是我们在用高级编程语言编写代码时往往认为理所当然的事情。在讨论细节之前，让我们先看一下客户代码。你可以通过在**memory/memoryguest**文件夹中创建一个新的Rust库板条箱来创建这个。

```rust
const MEMORY_BUFFER_SIZE: usize = 50;
static mut BUFFER: [u8; MEMORY_BUFFER_SIZE] = [0; MEMORY_BUFFER_SIZE];

#[no_mangle]
pub extern fn get_buffer_ptr() -> *const u8 {
  let pointer: *const u8 = {
    unsafe {
      BUFFER.as_ptr()
    }
  };
  pointer
}

#[no_mangle]
pub extern fn set_name(len: i32) -> i32 {
  let name = unsafe {
    std::str::from_utf8(&BUFFER[..len as usize]).unwrap()
  };

  let greeting = format!("Hello, {}", name);
  // Rust has a number of ways to do this more efficiently...
  // done "long hand" to illustrate what's happening
  unsafe {
    for (idx , byte) in greeting.as_bytes().iter().enumerate() {
      BUFFER[idx] = *byte;
    }
  }

  greeting.len() as i32
}
```

有一个单一的覆盖计算概念使所有这些代码成为可能:指针只是数字。如果指针只是数字，而WebAssembly模块只能交换数字，那么我们应该能够在客户机和主机之间传递指针，对吗?

这里的第一个导出get_buffer_ptr返回一个指向固定长度的静态可变数组的指针。在4.1实验之后，我们将更详细地讨论这个设计选择的复杂性。现在，主机可以使用这个指针作为写入共享线性内存数组的索引偏移量。一旦主机有了这个指针，它就可以将一个名称写入BUFFER数组所使用的内存段中。

第二个导出set_name实际上没有设置名称(此时主机已经将名称写入内存)。相反，它告诉客户名称已经设置，更重要的是，名称的长度已经设置。如果没有长度，guest模块就不能(轻松或可靠地)确定名称在缓冲区中的结束位置。这里隐含的但极其重要的一点是，名称的开头就是缓冲区的开头。

在下一页继续。
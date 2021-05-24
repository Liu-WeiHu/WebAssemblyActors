# 实验室2.2 -建造Rust Tic-Tac-Toe (1)

在前一章的第二个实验中，我们仅仅用一些捡来的浆果、一些稻草和原始的WebAssembly文本格式构建了一个井字游戏。在这个实验室里，我们要从废物升级为铁锈。

这个实验室复制了前一章实验室的相同特征，但这一次在Rust。通过这个练习是很重要的，这样您就可以了解如何在Rust中的WebAssembly模块中管理状态。

Rust是一种非常安全的语言。这样做的一个副作用是，如果您声明了一个全局可变状态，编译器将会抱怨并告诉您这是一个非常不安全的做法。大多数时候，我们会同意编译器的说法，并在全局可变状态下惊恐地运行。然而，我们知道一些编译器不知道的东西——我们正在编译的WebAssembly模块是单线程的，因此可变状态并不像编译器认为的那么糟糕。

有两种方法可以解决这个问题。首先，我们可以将全局可变状态包装在互斥锁或其他形式的线程同步或锁定原语中。我们知道我们永远不会遇到锁，但是机制仍然存在，以安抚编译器。另一种选择是承认这种状态固有的不安全特性，并使用不安全关键字将其标记为不安全。

在后面的章节中，当我们讨论角色模型时，我们将看到，在大多数情况下，我们不希望在WebAssembly模块中维护内部状态，但对于这些早期示例来说，这是可以的。以下是一些你可以用于井字棋实验的代码，显示了一些复杂的语法，用于声明、读取和写入全局可变状态:

```rust
#[derive(Debug, Clone, PartialEq, Copy)]
enum GamePiece {
  X = 2,
  O = 1,
  Empty = 0,
}

static mut GAME_BOARD: &'static mut [GamePiece] =
                               &mut [GamePiece::Empty; 9];

fn get_piece(index: usize) -> GamePiece {
  unsafe {
    GAME_BOARD[index]
  }
}

fn set_piece(index: usize, piece: GamePiece) {
  unsafe {
    GAME_BOARD[index] = piece
  }
}
```

您现在的工作是填写剩余的代码，并导出所有的功能，以先导。Go程序从第一章期望。当你修改Go模块指向你的第2章WebAssembly文件时，你就会知道你的实验室代码是有效的，游戏就像以前一样运行。为了满足Go runner，你需要导出以下函数:
- initGame
- takeTurn
- getPiece
- currentTurn
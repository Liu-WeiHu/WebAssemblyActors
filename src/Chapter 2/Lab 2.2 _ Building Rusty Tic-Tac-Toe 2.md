# 实验室2.2 -建造Rust Tic-Tac-Toe (2)

这些函数名不遵守Rust的函数命名约定。因此，如果希望导出与Go runner期望匹配的函数，可以使用export_name属性重命名导出的符号名，这是一个非常有用的技巧。

你的输出应该像这样:

```rust
#[no_mangle]
#[export_name = "initGame"]
pub extern "C" fn init_game() { ... }

#[no_mangle]
#[export_name = "takeTurn"]
pub extern "C" fn take_turn(row: i32, col: i32) { ... }

#[no_mangle]
#[export_name = "currentTurn"]
pub extern "C" fn current_turn() -> i32 { ... }

#[no_mangle]
#[export_name = "getPiece"]
pub extern "C" fn pub_get_piece(row: i32, col: i32) -> i32 { ... }
```

这段Rust代码中最棘手的部分可能是在何时何地使用不安全的块访问可变全局变量。说到全局变量，你可以使用全局可变状态声明来保存当前回合的状态:

<font face="微软雅黑">static mut TURN: GamePiece = GamePiece::X;</font>

当你完成了这个实验，你可以在运行中修改源文件名.go文件指向你新的，rust起源的WebAssembly模块:

```go
wasmBytes, _ :=
   ioutil.ReadFile(
       "/foo/target/wasm32-unknown-unknown/debug/tictactoe.wasm")
```

运行toerunner。Go file via Go run toerunner。Go会产生与你在第一章中创建的实验室相同的结果。
# 实验1.2_Tic-Tac-Toe

在这个实验中，你将结合你的知识控制流程指令，导入，输出，和使用线性记忆来创建一个井字游戏。你的模块应该导出以下函数:
- **initGame** 这个函数执行设置游戏板所需的任何步骤。一个新的游戏开始时是一个空的棋盘，第一个玩家总是“X”。
- **takeTurn** 这个函数接受两个i32参数，分别是行和列。你不需要处理复杂的错误处理，只要确保你的模块保持状态，这样它就知道什么时候转弯是“X”还是“O”。这个函数将推动当前玩家的回合，即X-&gt;O和O-&gt;X。
- **getPiece** 这个函数分别接受row和col的两个i32参数。它将返回一个i32，表示未被占用(0)、“X”(1)或“O”(2)。这个函数很方便，因为它意味着主机运行时不需要关心字节顺序或游戏板如何在内部存储。
- **currentTurn** 不接受任何参数并返回一个i32来指示当前回合是谁，X(1)还是O(2)。

线性存储器将被用来表示游戏板。X使用值“1”，Y使用值“2”(留下“0”表示未占用的空间)。你使用的内存应该是游戏棋盘的线性表示，所以从0到9的每个逻辑索引包含一个块，其中索引等于行* 3 + col。

注意，我们强调了逻辑索引，因为索引1和字节1之间有区别。线性内存是一个字节向量，但是您的数字都是32位整数(i32)，这意味着它们是4字节宽。load和store函数为您处理了端序性，但您仍然需要知道索引1位于字节位置/数组索引3。换句话说，要将逻辑数组索引转换为实际字节索引，需要将逻辑位置号(0-9)乘以4。

您将不能使用wasmtime来测试您的实验室代码，因为该工具没有保留模块来维护其状态。相反，我们在章节资源中提供了一个名为toerunner.go的Go文件。和你的tictactoe在同一个目录。wasm文件，运行Go文件开始游戏，如下图所示:

<font color=Blue>go run toerunner.go</font>
```text
Tic Tac Toe Runner
---------------------
.---.---.---.
|   |   |   |
|   |   |   |
|   |   |   |
`---^---^---'
[X's turn] enter row,col -> 1,1
.---.---.---.
|   |   |   |
|   | X |   |
|   |   |   |
`---^---^---'
[O's turn] enter row,col ->
```

如果你得到一个像这样的错误:

```text
cannot find package "github.com/wasmerio/wasmer-go/wasmer"
```

这只是Go抱怨你还没有下载这个包。为了解决这个问题，运行以下命令:

<font color=Blue>go get github.com/wasmerio/wasmer-go/wasmer</font>

然后重新运行go run命令。

你所创造的游戏不需要检测胜者，检测游戏何时结束，或阻止玩家占领之前占据的空间。为了额外的学分和更多的实践玩wat code，你可以添加这种功能通过额外的输出功能，然后你可以修改相应的Go主机的游戏循环。
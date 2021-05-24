# 实验室6.1 -创建协作Actors(奖金)

## Bonus: Use a Key-Value Store Provider

如果你有兴趣创建一个更真实的场景，你可以使用REPL或清单文件来将你的库存参与者链接到一个键值存储，比如一个本地Redis实例(看看[wasmCloud例子](https://github.com/wasmCloud/examples/tree/main/kvcounter) ，看看这是如何做到的)。然后可以修改库存参与者，从键值存储中读取每个SKU的当前库存数量，并作出相应的响应。
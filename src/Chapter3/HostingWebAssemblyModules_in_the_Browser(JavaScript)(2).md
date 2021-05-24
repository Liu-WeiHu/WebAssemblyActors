# 在浏览器中托管WebAssembly模块(JavaScript) (2)

由于浏览器的安全模型，简单地将web浏览器指向index.html可能无法工作。如果您将web浏览器指向http://localhost:8000，那么您应该能够打开web控制台(说明因浏览器而异)，并看到它正确地输出了值5。

这是您停下来并反思已经“艰难地”创建了WebAssembly模块的时刻之一，这样您就可以欣赏模块内所做的所有工作，这些工作仅仅是为了响应几行JavaScript代码。当你完成反思后，可以继续学习课程。

如果你正在使用Python web服务器，你会看到它处理对WebAssembly文件的请求:

```text
127.0.0.1 - - [28/Dec/2020 10:52:44] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [28/Dec/2020 10:52:44] "GET /index.js HTTP/1.1" 304 -
127.0.0.1 - - [28/Dec/2020 10:52:44] "GET /calc.wasm HTTP/1.1" 304
```

在完成取回承诺之后，我们只剩下一个JavaScript对象，它有两个字段:module和instance。模块就像它的名字暗示的那样，而实例是一个对象，它看起来非常像我们在本章开始时讨论的抽象运行时结构中的模块实例组件。

完成JavaScript文件index.js，这样我们就可以执行所有的模块导出:

```javascript
let importObject = {};
WebAssembly.instantiateStreaming(fetch('./calc.wasm'), importObject)
.then(obj => {
  console.log(obj.instance.exports.add(2,3));
  console.log(obj.instance.exports.mul(2,3));
  console.log(obj.instance.exports.div(10,2));
  console.log(obj.instance.exports.sub(10,2));
});
```

JavaScript的动态特性让我们可以把模块导出的函数当作实例对象原型的常规部分来处理。正如您将在接下来的几节中看到的，我们不像Rust和Go那样需要手动按名称查找函数。

现在不要担心空的importObject。我们将在本章后面讨论如何使用和管理导入。正如我们在课程的前面所述，您必须记住导入和导出总是相对于WebAssembly模块而不是主机。
# 在浏览器中托管WebAssembly模块(JavaScript) (1)

有一个短语可以用来描述目前关于让更多的人接受WebAssembly的力量的想法，“为浏览器而来，为云而待”。这指的是，很多人第一次接触WebAssembly是为了它在浏览器领域的潜在应用，但是经过一些教育和试验后，人们很快意识到WebAssembly的功能远远超出了浏览器的狭窄范围。

符合“为浏览器而来，为云而留”,我们会选出第一个(我们的独家使用wasmtime工具在前面章节)接触WebAssembly主机运行时是web浏览器,我们将向外(向上)云计算和超越。

如果您想在浏览器中使用WebAssembly进行大量开发，那么您需要熟悉Mozilla development Network的[WebAssembly引用](https://developer.mozilla.org/en-US/docs/WebAssembly) ，其中包括顶级JavaScript [WebAssembly对象](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/WebAssembly) 。

让我们从一个简单的例子开始:在浏览器中运行前一章的计算器模块。首先，让我们创建一个简单的HTML文件来保存我们的JavaScript代码:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="utf-8">
</head>

<body>
  <span id="container"></span>
  <script src="./index.js"></script>
</body>
</html>
```

这使得我们可以更容易地对JavaScript代码进行更改，而不必费心处理HTML标记。让我们创建第一个index.js文件:

```javascript
let importObject = {};
WebAssembly.instantiateStreaming(fetch('./calc.wasm'), importObject)
.then(obj => {
  console.log(obj.instance.exports.add(2,3));
});
```

这段代码使用fetch API来检索calc.wasm文件。在本章中，我们使用的是前一章的calc项目的发布版本，它导出了以下四个函数:add、sub、mul、div。

WebAssembly文件与您可能试图通过fetch获取的任何其他资源一样——它受到相同的安全性和跨域限制规则的约束。如果你有一个包含index.html, index.js和calc.wasm的单一目录，那么你所需要做的就是启动一个承载该目录的web服务器。你可以使用nginx这样的服务器，你最喜欢的web服务器，或者你可以使用下面的python3命令:

<font color=Blue>python3 -m http.server 8000</font>

```text
Serving HTTP on 0.0.0.0 port 8000 (http://0.0.0.0:8000/) ...
```

在下一页继续。
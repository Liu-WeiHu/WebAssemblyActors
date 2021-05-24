# waPC协议(2)

对于好奇的人来说，下面是该协议的确切功能列表。客户机总是从名为wapc的模块中导入，并且大多数导出的函数名前面都有两个下划线，以避免与其他导出的函数发生意外冲突。

|Function |Owner |Description|
|:-|:-:|-:|
|wapc.__console_log |Host|	客户调用主机登录到主机的标准输出(如果允许的话)|
|wapc.__host_call |Host|	客户呼叫请求主机执行操作|
|wapc.__host_response |Host|	告诉主机存储响应的指针地址|
|wapc.__host_response_len |Host|	返回主机响应的长度|
|wapc.__host_error |Host|	告诉主机存储错误块的指针地址|
|wapc.__host_error_len |Host|	返回主机错误团的长度|
|wapc.__guest_response |Host|	由来宾调用，以设置来宾的响应blob的指针和长度|
|wapc.__guest_error |Host|	由来宾程序调用，以设置来宾程序错误团的指针和长度|
|wapc.__guest_request |Host|	由来宾调用，以告诉主机在哪里存储请求操作(字符串)和请求有效负载(blob)值的指针地址|
|__guest_call |Guest|	由主机调用，以告诉来宾开始处理函数调用。然后，客户机将通过主机调用检索参数并设置响应值。|
|wapc_init |Guest|	在任何其他函数调用之前被主机调用(例如在启动/加载时)，让模块有机会执行任何必要的初始化|

幸运的是，你不需要自己实现这些，因为你可以在[wasmcloud GitHub](https://github.com/wasmCloud) 上找到Rust和Go的主机运行时库，以及针对AssemblyScript、Rust、TinyGo和Zig的客户库和代码生成工具。
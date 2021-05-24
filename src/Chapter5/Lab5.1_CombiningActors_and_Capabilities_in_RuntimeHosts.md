# 实验室5.1 -在运行时主机中结合角色和功能

在这个实验中，您将利用前面代码示例中用于构建参与者的技术。创建一个“booklibrary”actor(有趣的事实:调用板条箱库可能会导致奇怪的编译失败)，该actor通过一个HTTP服务器(例如“RESTful”)在一个库上公开一个CRUD(创建/检索/更新/删除)接口，允许用户创建新的图书条目、查询现有图书、更新图书和删除图书。

您不需要公开所有图书列表的查询，因为REST接口将由ISBN号驱动:

|Method |Path |Description| 
|:-|:-:|-:|
|POST   |/books         |	Store/create 有效负载中给定的图书|
|GET    |/books/{ISBN}  |	检索由ISBN标识的一本书;对于不存在的书返回404|
|PUT    |/books/{ISBN}  |	更新现有的图书，为不存在的图书返回代码404| 
|DELETE |/books/{ISBN}  |	删除一本书|

key-value存储的Book应该是一个从以下Rust结构序列化的JSON字符串:

```rust
#[derive(Serialize, Deserialize, Clone, Debug)]
struct Book {
    pub isbn: String,
    pub title: String,
    pub description: String,
    pub price: u32,
}
```

您应该能够从前面的代码示例中获得帮助来构建这个实验室。您可以使用清洗工具来启动和配置您的提供程序和库参与者。对于资源，您将希望查看wasmCloud的[键-值](https://github.com/wasmCloud/actor-interfaces/tree/main/keyvalue/rust) 和[HTTP服务器](https://github.com/wasmCloud/actor-interfaces/tree/main/http-server/rust) 参与者接口的代码/文档。
# 实验室3.2 -有趣的导入和导出

在本实验中，您将试验导入和导出，并创建另一个自定义主机。我们要在计算器上加上一个新函数，面积。有一个有趣的规则是，[任何形状的面积都可以由任何线段的平方来计算](https://betterexplained.com/articles/surprising-uses-of-the-pythagorean-theorem/) 。也就是说，Area = Factor * (line segment) ^ 2。下表显示了代入该公式的因子，以及线段使用的值:

|Shape| Line Segment| Area |Factor|
|:-|:-:|:-:|-:|
|Square |Side (s)|s^2 |1|
|Circle |Radius (r)	|π * r^2|	π|

还有更多的形状，但是为了保持这个实验室是关于WebAssembly而不是关于数学的，您的代码将只负责两个形状。在本实验中，在WebAssembly来宾模块中创建一个名为area的函数，它接受两个参数:

- line_segment (i32)

    用于计算面积的线段的长度。 
- shape (i32)

    这是一个表示形状的数字。使用1表示正方形，2表示圆形，而-1表示传递给形状的任何其他值。

这个函数返回一个类型为f32的值。

出于实验室的目的，假设来宾模块必须依赖主机来获得常数π的值。因此，当您的代码被要求计算圆的面积时，它将调用数学模块pi的导入，该模块返回类型为f32的值。您构建的主机应该为正方形、圆形和不支持的形状值调用area函数。
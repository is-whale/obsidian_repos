**以设计分数类为例，列举了分数计算中两种不同的处理方式，即下面例子中将 f 转为double 类型以及将4转为Fraction类型的两种方法**

Fraction f ( 3,5);

double d = f + 4;

![[images/Untitled 35.png|Untitled 35.png]]

### 方法一：将f转换为double类型

### 方法二：将4转换为Fraction类型

当两者共同存在时，调用d = f + 4报错，因为编译器不能选择使用何种方式。

### 方法三：

exolicit 明确的。 绝大部分情况用于构造函数前.

此处表示只有当明确使用构造函数的时候才调用，即阻止了4转换为Fraction类型。

![[images/Untitled 1 14.png|Untitled 1 14.png]]

可以看到阻止了4的类型转换。

### 使用举例（标准库）

![[images/Untitled 2 11.png|Untitled 2 11.png]]

**代理 设计模式（TODO:超链接）**

reference部分使用了代理设计模式，即使用reference代替了bool类型的返回值，那必须提供向reference类型的转换函数，即下面的bool()重载函数。
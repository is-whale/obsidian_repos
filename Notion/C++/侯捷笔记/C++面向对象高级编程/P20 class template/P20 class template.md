设计类时允许使用者把一些类型抽出来任意指定，如下面复数的实部虚部,可以指定任意类型

![[images/Untitled 36.png|Untitled 36.png]]

P20

![[images/Untitled 1 15.png|Untitled 1 15.png]]

min（）函数为模板函数，编译器事先不知道T的类型，在调用min的时候编译器会进行实参推导，判断出T的类型。

然后寻找<的重载

模板通常写在.h或.cpp，本身可以编译，但是在使用的时候编译器会再次编译。

P21

![[images/Untitled 2 12.png|Untitled 2 12.png]]
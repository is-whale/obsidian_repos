用到了std::bing。记录如下

原始调用

```C++
std::bind(&ProtocolDectctor::ParseCANFrame, this, std::placeholders::_1));
//将可调用对象与其参数一起进行绑定
```

解析:

- 类似延迟计算。 比如，回调函数，将回调函数传入后，回调函数不一定马上被调用。
- 它是一个模板类，调用后将生成一个新的调用对象A。调用该对象A与调用原函数是等价的。
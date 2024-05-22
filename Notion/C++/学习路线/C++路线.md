第一阶段：你学会了 C with Classes，然后把各种东西都包装成了 class；

第二阶段：为了实现多态，你学会了继承、[虚函数](https://www.zhihu.com/search?q=%E8%99%9A%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)、多继承和虚继承，然后你用这些技术改写了一些代码，实现了代码重用。你觉得很开心，感觉自己减少了代码量，提高了工作效率；

第三阶段：你学会了用[抽象类](https://www.zhihu.com/search?q=%E6%8A%BD%E8%B1%A1%E7%B1%BB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)作为接口，发现以前的继承关系太复杂，用接口更清晰，于是把代码都改成了单继承+接口。你觉得很开心，觉得自己设计了很好的架构；

[第四阶段](https://www.zhihu.com/search?q=%E7%AC%AC%E5%9B%9B%E9%98%B6%E6%AE%B5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)：为了实现和使用泛型容器，你学会了模板。你觉得很开心，又进一步提高了代码重用度；

第五阶段：你了解到了动态分派（Dynamic Dispatch）和静态分派（Static Dispatch），于是你把接口都改成了模板。你觉得很开心，不降低抽象程度却提高了代码执行效率；

第六阶段：你学会了异常。你觉得有点不爽，异常虽方便，但异常安全（[exception safety](https://www.zhihu.com/search?q=exception%20safety&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)）太难做到了；

第七阶段：你学会了[移动语义](https://www.zhihu.com/search?q=%E7%A7%BB%E5%8A%A8%E8%AF%AD%E4%B9%89&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)，你觉得很开心，可以用值的形式写出更高效的代码；

第八阶段：你学会了 unique_ptr，shared_ptr。你觉得很开心，妈妈再也[不用担心我](https://www.zhihu.com/search?q=%E4%B8%8D%E7%94%A8%E6%8B%85%E5%BF%83%E6%88%91&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)写出异常不安全的代码了；

第九阶段：你注意到了 Rust 这个语言，然后有意无意地在自己的 C++ 代码中贯彻 Rust 的思想，比如多用移动语义、[trait](https://www.zhihu.com/search?q=trait&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)、const；

第十阶段：你学会了[模板元编程](https://www.zhihu.com/search?q=%E6%A8%A1%E6%9D%BF%E5%85%83%E7%BC%96%E7%A8%8B&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)，SFINAE，并成功地使用不到 30 行代码使[编译器](https://www.zhihu.com/search?q=%E7%BC%96%E8%AF%91%E5%99%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)输出了 4G 错误信息。然后你用模板实现了一套[类型安全](https://www.zhihu.com/search?q=%E7%B1%BB%E5%9E%8B%E5%AE%89%E5%85%A8&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)的 trait 系统。你觉得很开心，编译器的错误提示终于可以看了；

第十一阶段：你用 gcc 6 编译了代码，然后用你库的人：what the f*ck？？怎么全是 undefined reference？原来他用的是 gcc 4.8。你不得不用 gcc 4.8 重新编译。你发现 gcc 4.8 的 libstdc++ 的 ostringstream 没有实现移动[构造函数](https://www.zhihu.com/search?q=%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)；

第十二阶段：你准备把代码部署到服务器上，what the ffffffffffuuuuucccccccc...？？！怎么全是 segfault？？？！！原来服务器的系统是 CentOS 6，libstdc++ 版本太低不兼容 C++11……你不得不在服务器上装了 gcc 4.8。后来你学会了[静态链接](https://www.zhihu.com/search?q=%E9%9D%99%E6%80%81%E9%93%BE%E6%8E%A5&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D) libstdc++；

第十三阶段：你再也不想用 C++ 了。你读了《Object Oriented Programming in ANSI C》和《[Inside the C++ Object Model](https://www.zhihu.com/search?q=Inside%20the%20C%2B%2B%20Object%20Model&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%22196189709%22%7D)》，然后用 C 改写了全部代码，并实现了一套使用 virtual function 机制的动态分派系统；

第十四阶段：你用宏把动态分派全改成了静态分派，并在 C 里模拟了一套 trait 系统；

第十五阶段：你发明了一个编译到 C 的语言。

第十六阶段：Rust 大法好。
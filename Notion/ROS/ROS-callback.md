## [ROS-callback](https://www.cnblogs.com/leaver/p/16474454.html)

# **01.ros::Subscriber()关联回调函数**

首先，使用**ros::Subscriber()**进行消息订阅，但此处需要注意的是，我们写的诸如下面这句代码只是一个声明，程序并没有真正地执行回调函数，直到遇到ros::spin()或ros::spinOnce()

**ros::spin()**与**ros::spinOnce()**，都是ROS消息回调处理函数。这两个函数需要结合ros::Subscriber()（ROS消息订阅函数）来使用。

## **1.1 ros::spin()**

- ros::spin()用于循环且监听回调函数（callback）
- 程序执行到spin()的时候，就会一直在循环中；
- 监听回调函数的意思是，如果有callback函数（），那写一句ros::spin()在这里，就可以在有对应消息到来的时候，运行callback函数里面的内容。适用于写在程序的（**因为写在这句话后面的代码不会被执行**），适用于订阅节点，且订阅速度没有限制的情况。
    
    这个节点
    
    当然必须在节点中红，定义Subscriber()
    
    末尾
    

## **1.2 ros::spinOnce()**

- ros::spinOnce()用于监听反馈函数（callback）。只能监听反馈，不能循环
- 这个函数比较灵活，可以控制接收速度，一般与loop_rate.sleep()，ros::ok()等同时出现，用来控制处理回调函数的频率，并且没有消息就收来时，就会程序堵塞，不会占用CPU资源。

# **02.callback回调函数**

回调函数需要在定义ros::Subscriber()的时候，进行关联;

```C++
ros::Subscriber sub = nh.subscribe("my_topic", 1, callback);
```

回调函数的第一个形参必须是订阅话题的消息类型，并且形参应该是智能指针的形式；

```C++
voidcallback(const std_msgs::StringConstPtr& str){}
```

## **2.1 回调函数的定义**

声明方式有很多种，如下列举三种

```C++
voidcallback(const boost::shared_ptr<Message const>&);
voidcallback(const std_msgs::StringConstPtr&);
voidcallback(const std_msgs::String::ConstPtr&);
```

对于每一种消息，都由共享指针的typedef方式，所以std_msgs::StringConstPtr和std_msgs::StringPtr都是可以使用的；

## **2.2 Subscriber()中关联回调函数**

1. callback： 普通函数形式（只有一个消息参数）
    
    【重要】
    

```C++
voidcallback(const std_msgs::StringConstPtr& str){...}
ros::Subscriber sub = nh.subscribe("my_topic", 1, callback);
```

1. callback： class method 类方法形式（只有一个消息参数）
    
    【重要】
    

```C++
void Foo::callback(const std_msgs::StringConstPtr& message){}
Foo foo_object;
ros::Subscriber sub = nh.subscribe("my_topic", 1, &Foo::callback, &foo_object);
```

1. callback： functor object 【不常用】 (A functor object is a class that declares operator()在类中定义一个operator()操作符)

```C++
class Foo{
public:
    void operator()(const std_msgs::StringConstPtr& message){}
};
ros::Subscriber sub = nh.subscribe<std_msgs::String>("my_topic", 1,Foo());
```

## **2.3 在Subscriber()中，使用boost::bind()绑定函数**

对于只有一个消息参数的回调函数，一般不需要使用boost::bind()进行回调函数的绑定，

但是一般要在回调函数中进行更加复杂的处理的话，回调函数需要携带多个形参，这时必须使用boost::bind()绑定回调函数。

boost::bind()的使用方法，参考【Boost程序库完全开发指南version2 page454】

```C++
// 回调函数的定义中，有三个形参voidcallback02(const turtlesim::PoseConstPtr& msg, int x, int y){}
// subscriber
ros::Subscriber pose_sub = n.subscribe<turtlesim::Pose>("/turtle1/pose", 2, boost::bind(&callback02, _1, input_x, input_y));
```

说明：

在Subscriber()中，绑定回调函数`boost::bind(&callback02, _1, input_x, input_y)`回调函数callback02()含有三个参数，

其中占位符_1，表示将订阅话题的消息内容作为第一个参数传递给callback02(),可能由于不知道具体的实参名称，所以使用_1表示函数调用时后的真正实参；input_x,input_y是在本程序段中实际的参数，可以直接传入，作为回调函数的第二、第三参数；

所以回调函数的调用相当于：callback02(PoseMsg, input_x, input_y);
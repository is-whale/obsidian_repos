## 模块间的话题订阅关系

---

该结构图不包含矢量地图载入、定位、点云过滤、路面去除、仿真器bridge等节点。

![[images/Untitled 30.png|Untitled 30.png]]

## **pure_pursuit节点**

聊这个节点之前，先说一下autoware是如何进行路径规划的：autoware中存在若干路径规划的节点，但他们统一的模式是使用Waypoint[] waypoints的方式生成一个waypoints数组，每个数组元素包含的是一个waypoint类变量，该变量内容主要包括

`# global id int32 gid # local id int32 lid geometry_msgs/PoseStamped pose //位置 geometry_msgs/TwistStamped twist //方向 ......`

也就是说，autoware中的路径Lane，实际上是一众包含id、位置、方向的点的集合，暂时且不管这些点是如何生成的，[pure_pursuit](https://zhuanlan.zhihu.com/p/395868166/edit)节点的作用就是根据车辆当前位置和规划后的路径点—final_waypoints，依据该路径点集的位置和方向去计算车辆的控制参数。_这里说的沿着这些点运动，并不是刻板的依照每个点的位置，让车辆去踩这些点，因为每个车转弯半径有限，这样做不现实。接下来开始这个节点的分析：_

```Plain
首先是消息的订阅：分别是需要跟踪的路径点集、当前车辆位置、？、当前车速
sub1_ = nh_.subscribe("final_waypoints", 10, &PurePursuitNode::callbackFromWayPoints, this);//final_waypoints(type autoware_msgs::Lane).
sub2_ = nh_.subscribe("current_pose", 10, &PurePursuitNode::callbackFromCurrentPose, this);
sub3_ = nh_.subscribe("config/waypoint_follower", 10, &PurePursuitNode::callbackFromConfig, this);
sub4_ = nh_.subscribe("current_velocity", 10, &PurePursuitNode::callbackFromCurrentVelocity, this);
```

**程序执行过程：**

- **获取路径点集final_waypoints**

该话题来自全局或局部路径规划器，消息类型为autoware_msgs::Lane，主要参数为waypoint[] waypoints，是一个类似数组的结构，waypoints内按顺序存储有多个路径点，每个路径点主要参数为：

```JavaScript
Geometry::poseStamped //该参数主要表示路径点空间位置信息包括位置和方向，主要参数为：
	Pose::point(x,y,z)	      //三维坐标参数
	Pose::quateration(x,y,z,w)    //四元数，三维方向参数	
	Geometry::TwistStamped //该参数主要表示在该点的速度信息，包括线速度和角速度，如下：
	Linear(x,y,z)	      //三轴线速度参数
	Angular(x,y,z)	      //三轴角速度参数
```

_如果使用Waypoints_saver这样的工具录制waypoints，则每个路径点的方向和速度都是录制过程中的真实参数；如果使用矢量地图规划路径，则每个路径点的方向由矢量地图决定，速度为一常量，加速度也为一常量，这两个参数可调。_

- **获取当前位姿**

话题名为/current_pose，消息类型为Geometry::poseStamped，同样主要信息为空间坐标和空间方向。

- **获取配置参数**
- **获取当前速度**

话题名为/current_velocity，消息类型为Geometry::TwistStamped，主要信息为三轴线速度与三轴角速度。

由于autoware车辆为阿克曼转向方式，虽然在速度参数定义时包含了三轴速度与角速度，但实际上只使用了x轴的线速度（车辆前进速度）和z轴的角速度（车辆水平转向），随后会加以分析。

- **设置预瞄距离,并选取next_point**

**next_point为下一个要跟踪的点，是以当前位置一定距离外的某一点。**

[![](https://pic1.zhimg.com/v2-3489602f579c95a456e028007deb5a24_r.jpg)](https://pic1.zhimg.com/v2-3489602f579c95a456e028007deb5a24_r.jpg)

pure_pursult算法原理示意图

`pp_.setLookaheadDistance(computeLookaheadDistance());`

可选恒定值或根据当前速度乘以某一常量计算。预瞄距离用于判断下一个waypoint值，与current_pose的欧氏距离大于**预瞄距离**的第一个点被设置为next_waypoint。

算法是按照id顺序，由近到远依次检索waypoint，距离小于预瞄距离则检索下一个点，直至某一点大于预瞄距离，停止向后检索，得到如图中绿色的点，随后的检索将从该点开始。

- **根据current_pose与next_point计算曲率**

`bool can_get_curvature = pp_.canGetCurvature(&kappa);`

这里的曲率指的是上图中橙色圆弧半径的倒数，**注意：这一圆弧才是真正的车辆行驶的轨迹**。阿克曼转向——前轮转角固定时，车辆行驶轨迹为圆弧。所以计算得到的曲率会应用于车转向的控制中。

```JavaScript
计算所用函数为：
double PurePursuit::calcCurvature(geometry_msgs::Point target) const
{
 double kappa;
 double denominator = pow(getPlaneDistance(target, current_pose_.position), 2);
 double numerator = 2 * calcRelativeCoordinate(target, current_pose_).y;
 if (denominator != 0)
 kappa = numerator / denominator;
 else
 {
 if (numerator > 0)
 kappa = KAPPA_MIN_;
 else
 kappa = -KAPPA_MIN_;
 }
 ROS_INFO("kappa : %lf", kappa);
 return kappa;
}
//calcRelativeCoordinate函数内为一通tf变换，呃，具体计算方法不清楚。
```

  

- **根据曲率计算车辆控制参数/twist_raw与/ctrl_cmd**

/twist_raw包含两个主要参数：twist.linear.x与twist.angular.z，其中twist.linear.x的值为一个常量，即初始化过程中设置的车辆最大行驶速度，(直线段和转弯处这个最大行驶速度值不一样)也是矢量地图中每个路径点所包含的速度值。而z轴角速度通过下面这个算法计算：

`twist.angular.z = twist.linear.x * kappa；//（曲率乘x轴线速度）。`

/ctrl_cmd包含3个主要参数：liner_velocity，linear_acceration，Steering_angle，前两个参数分别是线速度与线加速度，这两个都是固定值，加速度恒定，速度随加速度变化直至速度上限。这两个常量与/twist_raw内的速度、加速度常量值相同。Steering_angle为方向盘的转角，其中Steering_angle=arctan(wheel_base*kappa)，（转向常量*曲率的反正切函数值）。

_ps:程序里还有一部分是判断是否需要插值的部分，应该是应对路径陡然变化的特殊处理方式，有兴趣的自己看看吧，这里就不罗列了。_

- **/twist与/ctrl_cmd话题发布**

### 末端twist两个节点

Twist两个节点是autoware所有路径规划后的最后一个节点，用于将控制车辆运动的命令做修饰整合，并以一定的格式发布出去，当然依旧是依靠ros消息的方式。该消息名为/Vehicle_cmd，该消息控制车辆运动还需某一节点将ros消息转化为CAN、uart等数据进行车辆的直接控制，autoware并不提供该节点。

### Twist_fifter节点

订阅由pure_pursuit节点发布的/twist_raw话题，这一话题中包含两个变量，每个参数包含三个参数，对应三轴线速度和三轴角速度，但实际上只用到了控制车辆的x轴线速度和z轴角速度即车速与转角。

```JavaScript
/twist消息内容
Vector3  linear
Vector3  angular

/Vector3内容
float64 x
float64 y
float64 z
```

  

_twist_filter用于限定_x轴线速度和z轴角速度_这两个参数的值，大于限定值后更改参数为最大限定值，随后以/twist_cmd话题发出。_

## [**Twist_gate**](https://zhuanlan.zhihu.com/p/395868166/edit)**节点**

这个节点将会订阅所有关于车辆控制的话题，如/twist_cmd,/ctrl_cmd以及xxx_cmd等，随后将多种控制命令整合至/Vehicle_cmd话题输出。

`/Vehicle_cmd话题变量内容包括： Header header autoware_msgs/SteerCmd steer_cmd autoware_msgs/AccelCmd accel_cmd autoware_msgs/BrakeCmd brake_cmd autoware_msgs/LampCmd lamp_cmd int32 gear int32 mode geometry_msgs/TwistStamped twist_cmd autoware_msgs/ControlCommand ctrl_cmd int32 emergency`

源码中存在3套车辆的运动控制指令，应该是为了适用不同系统而设置。

```JavaScript
 1.
          steer_cmd 方向命令
          accel_cmd 加速度命令
          brake_cmd 刹车命令
          lamp_cmd  车灯命令
          gear      档位命令(D P N .etc)
          mode      模式命令(auto,remote)

 2.twist_cmd 仅有两个参数
       	  twist.linear.x    x轴线速度(车辆前方为x轴正方向)
          twist.angular.z   z轴角速度(车辆正上方为z轴正方向)

 3.ctrl_cmd 三个参数
          linear_velocity               线速度
          linear_acceleration \#m/s^2    线加速度
          steering_angle                转向角
三套控制命令相互独立，可订阅话题后任选一种使用。
需要确认发送的是哪种控制格式
```
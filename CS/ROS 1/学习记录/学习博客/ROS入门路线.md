我来答一波这个问题，目前某西北某985高校大四[自动化专业](https://www.zhihu.com/search?q=%E8%87%AA%E5%8A%A8%E5%8C%96%E4%B8%93%E4%B8%9A&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%221241736983%22%7D)学生，学ROS已经一年有余。

刚开始也纠结过这个问题，毕竟搭建一个底盘，先不说硬件价格，操作还是比较麻烦的。虽说ROS wiki（[https://www.ros.org/](https://link.zhihu.com/?target=https%3A//www.ros.org/)）上的[开源社区](https://www.zhihu.com/search?q=%E5%BC%80%E6%BA%90%E7%A4%BE%E5%8C%BA&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%221241736983%22%7D)上提供了大量功能包，各种博客上也包含了各种快速搭建的教程和方法，但仍然躲不开几个问题：编码器的驱动怎么配置？底盘驱动程序怎么写？上位机和下位机怎么进行通信？如何把整个系统耦合到ROS中，比如如何利用/cmd_vel话题控制底盘运动？miniPC，激光雷达，各个传感器的单独供电怎么解决？需不需要升压电路？这里只举了几个可观的例子，不再赘述。（要不然各个高校实验室也不会买那么多turtlebot是吧...）

**刚开始**的时候是一定要先在自己电脑上仿真的，节省精力和时间成本。必然要看一遍古月的课or中科院软件所那个也挺好（源于中国大学慕课的风格，每节课程教学短小精悍），大致了解一些ros下的基本命令，三种通讯方式怎么调怎么自定义，如何改工作空间里的CMakelists，细节到如何配置OpenCV（主要是怎么改cmake加头文件把opencv的API引进工程里），如何安装navigation功能包和配置一些建图算法Gmapping，hector，karto等。

[sychaichangkun/ROS-Academy-for-Beginners](https://link.zhihu.com/?target=https%3A//github.com/sychaichangkun/ROS-Academy-for-Beginners)[github.com/sychaichangkun/ROS-Academy-for-Beginners](https://link.zhihu.com/?target=https%3A//github.com/sychaichangkun/ROS-Academy-for-Beginners)

[【古月居】古月 · ROS入门21讲_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1zt411G7Vn%3Ffrom%3Dsearch%26seid%3D14202911098539103595)[www.bilibili.com/video/BV1zt411G7Vn?from=search&seid=14202911098539103595](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1zt411G7Vn%3Ffrom%3Dsearch%26seid%3D14202911098539103595)

[中科院软件所-机器人操作系统入门（ROS入门教程）_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1mJ411R7Ni%3Ffrom%3Dsearch%26seid%3D14202911098539103595)[www.bilibili.com/video/BV1mJ411R7Ni?from=search&seid=14202911098539103595](https://link.zhihu.com/?target=https%3A//www.bilibili.com/video/BV1mJ411R7Ni%3Ffrom%3Dsearch%26seid%3D14202911098539103595)

**之后要达到什么程度呢**，你要你能把你学到的功能：比如建图，定位导航，[cv识别](https://www.zhihu.com/search?q=cv%E8%AF%86%E5%88%AB&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%221241736983%22%7D)，语音识别等拼接起来。比如在gazebo的仿真环境里（以中科院那个虚拟环境为例 ⇒\Rightarrow 第一个链接)，你能实现从大厅走到教研室，识别出那个屋子里面是椅子or桌子or黑板or电脑，返回原点，输出这个信息：教研室有一个椅子）。这些其实并不是难事，你不需要改各个功能的具体代码，ROS的控制方式是点对点的，各个节点都是松耦合的不会互相影响，你只需要让各个node之间以ROS间通信的形式互相触发就可以了。

ROS navigation功能包的流程图镇楼

学到这个程度，你的ROS水平足够了，但还是对具体算法一无所知，但这起码保证了你拿到实体机器人后可以快速上手。因为答主说了以后想搞SLAM，不是嵌入式，所以拿到的机器和你之前仿真的区别应该就是gazebo那个节点，改成了底盘驱动控制器，话题名字可能也有轻微改变。半个小时上手没问题，不多赘述。如何学SLAM，可以看我另一个回答：

[硕士研究生阶段如何学习slam机器人？1303 赞同 · 77 评论回答](https://www.zhihu.com/question/396119527/answer/1235876702)

看到这我废话了这么多，这里可以休息下了^_^

---

分享给题主一个我借助Turtlebot3的一系列功能包，自己搭建的一个学Cartographer的仿真环境~

先附仓库地址：

[https://github.com/xuankuzcr/Cartographer_ICRA](https://link.zhihu.com/?target=https%3A//github.com/xuankuzcr/Cartographer_ICRA)[github.com/xuankuzcr/Cartographer_ICRA](https://link.zhihu.com/?target=https%3A//github.com/xuankuzcr/Cartographer_ICRA)

- **配置好cartographer** （[carographer cartographer_ros cartographer_turtlebot ceres-solver](https://link.zhihu.com/?target=https%3A//github.com/googlecartographer)）
- **安装依赖项**

`sudo apt-get install ros-kinetic-joy ros-kinetic-teleop-twist-joy ros-kinetic-teleop-twist-keyboard ros-kinetic-laser-proc ros-kinetic-rgbd-launch ros-kinetic-depthimage-to-laserscan ros-kinetic-rosserial-arduino ros-kinetic-rosserial-python ros-kinetic-rosserial-server ros-kinetic-rosserial-client ros-kinetic-rosserial-msgs ros-kinetic-amcl ros-kinetic-map-server` [`ros-kinetic-move-base ros-kinetic-urdf`](https://www.zhihu.com/search?q=ros-kinetic-move-base%20ros-kinetic-urdf&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%221241736983%22%7D) `ros-kinetic-xacro ros-kinetic-compressed-image-transport` [`ros-kinetic-rqt`](https://www.zhihu.com/search?q=ros-kinetic-rqt&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%221241736983%22%7D)`-image-view ros-kinetic-gmapping ros-kinetic-navigation ros-kinetic-interactive-markers`

- **获取源码放到工作空间下**

`mkdir -p ~/catkin_ws/src/ && cd ~/catkin_ws/src/ sudo cp ~/Cartographer_ICAR/{` [`turtlebot3`](https://www.zhihu.com/search?q=turtlebot3&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%221241736983%22%7D)`,turtlebot3_msgs,turtlebot3_simulations,waterplus_map_tools} ~/catkin_ws/src/`

- **编译**

`cd ~/catkin_ws catkin_make echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc echo "export TURTLEBOT3_MODEL=burger" >> ~/.bashrc`

- **使用说明：**为了方便使用我把launch文件合成了shell脚本放在仓库目录的shell_carto文件夹内

**1. cartographer建图**

打开仿真环境

`cd shell_carto/ ./gazebo_empty_start.sh`

[![](https://picx.zhimg.com/80/v2-0e7c046caf02e061c4b11e485d0cf077_720w.webp)](https://picx.zhimg.com/80/v2-0e7c046caf02e061c4b11e485d0cf077_720w.webp)

Cartographer建图

`./mapping_cartographer.sh`

保存地图

`mkdir ~/map ./savemap.sh`

**2. 利用 cartographer纯定位+movebase 进行定点导航**

`./gazebo_empty_start.sh ./carto_localization.sh`

**3. 基于2实现多点连续导航**

利用Add Waypoint设置多个目标点

`sudo cp waypoints.xml ~/waypoints.xml ./gazebo_empty_start.sh ./tools_carto_localization.sh`

保存标注目标点信息到~/waypoints.xml,并依次发送move_base_msgs::MoveBaseGoal goal信息

`./map_tools.sh`

如果想自己diy一个底盘玩玩的话推荐这个[ros_arduino_bridge](https://www.zhihu.com/search?q=ros_arduino_bridge&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A%221241736983%22%7D)，下位机程序不用写，有个16进制文件可以直接烧进去。Realsense可以买一个跑个orb玩玩，Intel这个RGBD摄像头教程多便宜效果还不错。

我们社团现在已经买了这个了，再也不用自己搞底层了，可惜已经要毕业了...

[![](https://picx.zhimg.com/80/v2-4b88c0e2cb06f2999d11597853f8ab8d_720w.webp)](https://picx.zhimg.com/80/v2-4b88c0e2cb06f2999d11597853f8ab8d_720w.webp)
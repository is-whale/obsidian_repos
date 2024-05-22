## 1.前言

航迹点用于指导汽车运行，autoware的每个航迹点包含x, y, z, yaw, velocity信息。

航迹点录制有两种方式，可以开车录制航迹点，也可以采集数据包。

**所有需要在 [Simulation] 菜单下加载的数据，都需要在所有操作之前操作，否则在RViz显示时，会出现frame_id错误。**

## 2. 仿真数据录制航迹点

航迹点是车在运动过程中，车载lidar与地图匹配形成的一系列的位置信息构成的。所以定位就是录制航迹点的基础。

2.1 打开runtime manager

2.2 打开地图要定位的数据

进入 [Simulaton] 页面，点击界面右上方 [Ref] 按钮，加载录制用于定位的 bag 文件。

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706145752982-1405141324.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706145752982-1405141324.png)

点击 [Play] 然后点击 [Pause]暂停。

2.3 加载地图，加载world到map以及base_link到velodyne的TF变化

(1) 设置从base_link到velodyne坐标系的TF

在 [Setup] 菜单中，确保 [Localizer] 下选项为 [Velodyne]，在 [Baselink to Localizer] 中设置好各个参数之后点击 TF 按钮，其中x、y、z、yaw、pitch、roll表示真车雷达中心点与车身后轴中心点的相对位置关系（右手坐标系，真车后车轴为原点），此时可以点击[Vehicle Model]，如果[Vehicle Model]为空，那么会加载一个默认模型（在rviz显示时，如果有激光雷达数据，车辆会显示为黑色）。如下所示。

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706110515888-778146035.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706110515888-778146035.png)

（2）设置从world到map转换

点击 [Map] 页面，点击 [TF] 的 [ref] 选择 autoware/ros/src/.config/tf/tf_local.launch 文件，这是加载默认world到map的坐标转换，打开tf_local.launch文件如下：

```JavaScript
<launch>
    <node pkg="tf" type="static_transform_publisher" name="world_to_map" args="0 0 0 0 0 0 /world /map 10" />
</launch>
```

  

args的参数“0 0 0 0 0 0 /world /map 10”表示：从/world坐标系转换到/map坐标系的x, y, z, roll, pitch, yaw转换，且频率为10Hz。

点击 [TF] 按钮，如下图：

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706112236387-129672415.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706112236387-129672415.png)

（2）加载地图

选择runtime manager的 [Map] 菜单，点击 [Point Cloud] 按钮的 [ref]，加载培训笔记 No. 1产生的.pcd文件（点云地图），并点击 [Point Cloud] 按钮，进度条显示OK，则加载完毕，如下所示：

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706133830675-826233972.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706133830675-826233972.png)

2.4 设置相关滤波器（No. 2重复）

选择 [Sensing] 页面，点击 [Points Downsampler] 下 [voxel_grid_filter] 的 app，设置一些参数，[voxel_grid_filter] 是一种将采样方法，将点云数据用质心近似（用于降采用），如下图：

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190704224755681-1075879273.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190704224755681-1075879273.png)

这里，[Voxel Leaf Size] 参数值为2，意义是2米的立方体内的全部点近似用1个质心代替。[Measurement Range]的参数值为2，意义是点云的有效的距离为200米。

2.5 设置从map到base_link的转换（NDT_MATCHING）（No. 2重复）

找到 [Computing] 左菜单栏下的 [ndt_matching] 选项，打开 [app] ，如下所示：

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706153648958-1648815743.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706153648958-1648815743.png)

确保 [topic:/config/ndt] 选项处于 [Initial_Pose] 处，勾选 [Initial Pos]，x，y，z，roll，pitch，yaw的值表示激光的初始位置，我们这里用默认的0，0，0，0，0，0。如果有GPU，那么[Method Type]可以选择pcl_anh_gpu用GPU帮助运算。其他参数用默认值。

2.6 启动[vel_pose_connect]（No. 2重复）

找到 [Computing] 左菜单栏下的 [vel_pose_connect] ，打开 [app] 并确保选项 [Simulation_Mode] 没有被勾，退出并勾选 [vel_pose_connect]。

2.7 记录航迹点

在菜单栏 [Computing] 右边的 [waypoint_maker] 下的 [waypoint_saver]。点击 [app] 按钮，在打开的界面中点击 [Ref] 指定保存路径和文件名后点击 [OK] 。如下所示：

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706211956092-1962049715.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706211956092-1962049715.png)

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706212034172-458903976.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706212034172-458903976.png)

[app]在勾选框 [Save/current_velocity]，可以保存waypoint的速度，参数[Interval]中的值表示采点间隔，默认1m，可以修改为不同的值，我们用默认值效果还不错。退出并勾选 [waypoint_saver]， 如上图片右侧所示。

2.8 显示数据

打开 [Rviz], 在 [file] 菜单中的 [open config] 选择路径：autoware/ros/src/.config/rviz/default.rviz 的文件。这是回到runtime manager，进入 [Simulaton] 页面，点击 [Pause]开始。这时可以从RViz中看到一辆黑色的带有激光雷达数据的汽车停在地图的右下方，如下图所示：

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706182759467-133517187.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706182759467-133517187.png)

如果车子不在预想的位置上，那么点击RViz的工具 [2D Pose Estimate] 重新为车子选择上图的位置和方向。

2.9 在RViz显示waypoints记录过程

打开 [Rviz] ，在 Rviz 左下方找到 [Add]。弹出的显示框中找到图示 [By topic] 下 话题 /waypoint_saver_marker 的 [MakerArray] 并添加。如下所示：

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706212409300-1440852797.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706212409300-1440852797.png)

2.10 保存记录数据

返回 runtime manager 的 [Simulation] 菜单，点击 [Pause] 按钮，这时可以看到车子在地图中开始运动，车子停止后，回到 runtime manager的 [Computing] 菜单栏下，取消 [waypoint_saver] 勾选（自动保存waypoint）。

注意：确保在此之前更改了文件名和保存路径，在指定的路径下将会看到对应文件。

## 3. 实车录制航迹点

3.1 启动runtime_manager

3.2 启动激光雷达

（1）针对velodyne 多线lidar

velodyne激光可以启动默认配置：[Sensing] 页面下 [Lidars] 选项下针对你的velodyne选择，velodyne激光对应的发布的点云topic：/velodyne_packets，frame_id：velodyne，topic会被重映射成为/points_raw。

  

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190704100412724-497902238.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190704100412724-497902238.png)

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190704100318408-886667669.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190704100318408-886667669.png)

查看点云输出。

3.3 加载、设置参数

3.2 启动航迹点记录

3.3 移动车到要录制航迹点的起点

3.4 开启记录航迹点

3.5 打开RViz，查看定位

打开 [Rviz], 在 [file] 菜单中的 [open config] 选择路径：autoware/ros/src/.config/rviz/default.rviz 的文件。此时在地图上会显示真车在地图上的实际位置。如下所示：

[![](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706210609886-454061968.png)](https://img2018.cnblogs.com/blog/1023160/201907/1023160-20190706210609886-454061968.png)

与定位显示类似，但是有时候由于初始位置与 map TF 有一定的距离，所以导致定位晃动无法准确与地图绑定，这时需要借助 Rviz 中上菜单栏里的 [2D Pose Estimate] 箭头进行辅助定位。

3.6 在RViz显示waypoints记录过程

3.7 保存记录数据

到达目标后，回到 runtime manager的 [Computing] 菜单栏下，取消 [waypoint_saver] 勾选（自动保存waypoint）。
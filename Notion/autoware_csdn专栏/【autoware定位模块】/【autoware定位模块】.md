---
Created: 2023-12-18T14:10
Updated: 2023-12-18T14:25
URL: https://blog.csdn.net/qq_35635374/article/details/124540935
---
### 文章目录

- [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/124540935#_0)
- [前言](https://blog.csdn.net/qq_35635374/article/details/124540935#_13)
- [一、常见的定位方法概述](https://blog.csdn.net/qq_35635374/article/details/124540935#_24)
    - [1.常用的定位方式](https://blog.csdn.net/qq_35635374/article/details/124540935#1_25)
    - [2、定位的作用](https://blog.csdn.net/qq_35635374/article/details/124540935#2_44)
    - [3、建图时的定位与自动驾驶时的定位需求的异同](https://blog.csdn.net/qq_35635374/article/details/124540935#3_52)
- [二、一个简单的较通用的定位流程](https://blog.csdn.net/qq_35635374/article/details/124540935#_76)
- [三、定位相关的功能包解析](https://blog.csdn.net/qq_35635374/article/details/124540935#_88)
    - [1.【gnss_localizer的GPS定位解析功能包】](https://blog.csdn.net/qq_35635374/article/details/124540935#1gnss_localizerGPS_89)
    - [（1）GPS定位的作用](https://blog.csdn.net/qq_35635374/article/details/124540935#1GPS_90)[（2）GPS获取局部定位的原理](https://blog.csdn.net/qq_35635374/article/details/124540935#2GPS_94)[（3）模块介绍](https://blog.csdn.net/qq_35635374/article/details/124540935#3_98)[（4）关键函数分析](https://blog.csdn.net/qq_35635374/article/details/124540935#4_104)
    - [2.【Ndt_cpu的NDT算法原理功能包】](https://blog.csdn.net/qq_35635374/article/details/124540935#2Ndt_cpuNDT_112)
    - [（1）模块介绍](https://blog.csdn.net/qq_35635374/article/details/124540935#1_114)[（2）NDT算法框架的实现流程](https://blog.csdn.net/qq_35635374/article/details/124540935#2NDT_119)[（3）关键的函数解析](https://blog.csdn.net/qq_35635374/article/details/124540935#3_127)
    - [3.【NDT_matching定位集成功能包】](https://blog.csdn.net/qq_35635374/article/details/124540935#3NDT_matching_131)
    - [（1）模块介绍](https://blog.csdn.net/qq_35635374/article/details/124540935#1_132)[（2）关键的函数分析](https://blog.csdn.net/qq_35635374/article/details/124540935#2_143)
- [四、【在高精度地图中实现组合定位】](https://blog.csdn.net/qq_35635374/article/details/124540935#_156)
- [总结](https://blog.csdn.net/qq_35635374/article/details/124540935#_169)

## 一、常见的定位方法概述

### 1.常用的定位方式

**1、GPS/RTK差分定位**

（绝对型的定位，精度较高，但是成本很高）

**2、基于高精度地图的激光slam匹配（点到点、点到线、点到面的）定位算法**

load、ndt（存在累计误差，长时间会产生定位漂移）

**3、基于高精度地图vector_map的语义信息定位**

（根据vector_map的路标和车道线id、更多的是基于视觉融合定位【高精度地图还可能包含图像信息，这就可以通过实时图像与高精度题图图像匹配融合得到定位信息–匹配语义信息】）

矢量地图vector_map，全局粗略信息定位，vector_map的link_id定位跑到哪一条道上

**4、轮速里程计**

（存在累计误差，长时间会产生定位漂移）

根据场景组合使用，这称为多传感器融合定位

### 2、定位的作用

定位是为了能够准确获取车辆的位置、姿态的变化

1）第一用途：离线定位是为了建立地图

2）第二用途：在线定位是主要为了决策规划、感知

### 3、建图时的定位与自动驾驶时的定位需求的异同

**区别**

1、建图过程中的定位可以离线进行，不要求定位的实时性（定位用于回环检测，建立全局图的过程可以是离线的）

2、建图过程中的定位仅仅参考已经建立的地图和当前的传感器数据进行计算

3、建图过程中的定位的起始位置、终点位置是通过给定的

4、自动驾驶过程中的定位是必须在线实时提供定位信息的（无论时rtk、GPS、还是ndt_matching）

5、自动驾驶过程中的定位是综合参考高精度地图和外部定位GPS传感器数据进行计算

6、自动驾驶过程中的定位的起始位置、终点位置是随机给定的，因此需要先验的GPS/RTK给定一个初始的位置

**相同点**

1、自动驾驶和建图的定位后端优化的原理是类似，都是loam的一些方案

2、定位和建图所需要的硬件是类似的，定位是建图的前提，换句话说，能建图一定能定位【所以定位是一个方向，地图也是一个方向，但是两者在某一方面是可以相辅相成的，建立地图必须依赖定位，激光雷达定位也依赖地图，但是绝对型定位RTK、gps、uwb，视觉里程计等不需要依赖地图】

.

.

## 二、一个简单的较通用的定位流程

![[post-images/cbad99e55ae74a2fa6baf0397fa13234.png]]

1、车辆启动的时候，GPS/RTK提供一个定位初始位置

2、定位初始位置给到NDT的匹配算法，计算出相对的的定位

3、扩展的定位工作：GPS/RTK、轮式odom、视觉odom、惯导积分也是在同时进行的【我没有进行定位的进一步研究】

4、在车辆行进过程中，根据场景特点与定位算法的漂移参数fitness_core特点，不断切换GPS/RTK、NDT的匹配算法或者其他定位方案，实现定位的多传感器融合

（DNT算法计算的位置突然变化，或者出现累计误差的时候，GPS可以直接重启纠正回来（提高定位的安全性吧）【fitness_core决定了GPS介入纠偏的条件，fitness_core一般设置为7，fitness_core参数过小，重新计算量会大一点且定位一直在抖动，fitness_core参数过大，长时间跑位置可能不太对】

使用GPS进行纠偏的时候首先要保证GPS是准的，一般在空旷的地方NDT才不准，但是在空旷的地方NDT特别准，在室内或有遮挡的时候gps不准，但是NDT特别准）

.

.

## 三、定位相关的功能包解析

### 1.【gnss_localizer的[GPS定位](https://so.csdn.net/so/search?q=GPS%E5%AE%9A%E4%BD%8D&spm=1001.2101.3001.7020)解析功能包】

### （1）GPS定位的作用

Gnss_localizer主要是把GPS的经纬高度信息转换成为xy坐标系，GPS主要用于辅助定位，输出一个/gnss_pose的话题（转换的过程中仅仅需要指定一个原点），在开阔环境室外场景下，一开始的是GPS提供一个初始值，匹配DNT初始位置，在定位的过程中作为辅助定位的方式进行纠偏

.

### （2）GPS获取局部定位的原理

通过GPS和rtk提供经纬高度的信息，根据给定的基于基站的原点解析成一个world坐标系的xyz（球模型，或者椭球模型–具体转换原理可以不理解），在实际的地块中，我们根据地图和地块的坐标系（地图和地块的坐标系是重合的），给定车的起点为原点，得到地块坐标系下的xyz坐标（就是坐标系平移的原理）

.

### （3）模块介绍

![[post-images/b079b66a8f9d4189859caffe8ab67a53.png]]

（1）Nmea2tfpose这一个节点实际是GPS的一个使用协议的解析功能，用于解析GPS的字符串信息，提取出经纬高、欧拉角，得到/gnss_pose的作用

（2）fix2tfpose这一个节点是Nmea2tfpose的一个子集，仅仅能够解析出来经纬高

选用什么样的功能包主要取决于用什么样的传感器【不是所有的GPS模块都会有欧拉角rpy的姿态信息，但是xyz的位置信息肯定是有的，但是我们实际也可以通过相邻的经纬度推算出来位姿yaw角的，有两个天线就能计算位姿，只有一条天线只能输出位置】

.

### （4）关键函数分析

![[post-images/8609431fde9d4792a36dccf8a848447c.png]]

这个功能包实在my_location.launch的启动文件中进行调用的

.

.

### 2.【Ndt_cpu的NDT算法原理功能包】

### （1）模块介绍

Ndt_cpu是NDT算法的实现，Ndt_cpu的算法难度比较大，需要先理解NDT论文和算法，然后对照论文论文理解代码的实现

![[post-images/483622b2a28c4895888d74f8c60bac45.png]]

.

### （2）NDT算法框架的实现流程

（1）输入一张点云地图map，把点云地图map分成多个小格子【这一个过程使用PCL的库函数】

（2）遍历每一个小格子，计算每个小格子的点云（xyz）均值与方差

（3）构建一个误差优化函数【使用的是概率密度函数作为误差优化函数】，并求解该误差优化函数（遍历每一个scan数据），求解得到各个矩阵–这个过程本质原理可以理解成为卡尔曼滤波的过程

（4）输出优化的定位信息

.

### （3）关键的函数解析

![[post-images/f30aabad24924db286c49e6e986f47d9.png]]

### 3.【NDT_matching定位集成功能包】

### （1）模块介绍

Ndt_matching是定位的主线程，在室内场景下，点云地图pcd不仅仅是用于绘制vector_map矢量地图，还用以点云特征匹配来定位

![[post-images/68bfd1de0e6b42968cdeec05f9b0a375.png]]

模块流程步骤：

（1）【数据输入】首先GPS提供一个初始的定位位姿/gnss_pose

（2）【数据输入】通过先验的高精度地图得到原始的点云地图/points_map、原始的点云数据/points_raw（后面会进行下采样滤波的，滤除地面上无效的点）

（3）【数据输入】惯导IMU的位姿信息/imu、轮式里程计的信息/odommetry是可选的

（4）三位雷达定位的功能包lidar_localizer是一个虚功能包，包括NDT_matching和load的一些定位方案

（5）NDT_matching功能包主要是通过ndt_cpu这个功能包实现的，输出ndt算法的定位话题/ndt_pose

（6）/ndt_pose的定位数据被整个导航系统的各个模块进行订阅使用，包括感知、地图、决策规划、控制等模块

.

### （2）关键的函数分析

![[post-images/2431cd18cef24021b42d018df56f96c7.png]]

因为组合定位是用到各个传感器的定位信息，因此当ndt_matching定位发生偏移甚至是丢失时的时候，GPS也会进行纠正

.

.

## 四、【在高精度地图中实现组合定位】

启动地图，定位的对应节点和rosbag

![[post-images/ae1e5a24852243679ceaa887d0357246.png]]

---

## 总结

本文介绍通过点云数据和高精度地图获取定位数据

ndt既可以用于建图，也可以用于建图，实现边输出定位，边创建更新地图，ndt可以理解就是amcl的升级版

纯靠AMCL和NDT定位会出现位置误差的累积和发散，长时间运行定位回发散，必须使用[GNSS](https://so.csdn.net/so/search?q=GNSS&spm=1001.2101.3001.7020)和RTK进行辅助定位
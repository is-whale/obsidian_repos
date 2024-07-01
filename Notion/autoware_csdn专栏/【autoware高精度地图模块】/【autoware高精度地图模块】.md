---
Created: 2023-12-18T14:11
Updated: 2023-12-18T14:26
URL: https://blog.csdn.net/qq_35635374/article/details/124539162
---
**剪藏来源:** [【autoware高精度地图模块】-CSDN博客](https://blog.csdn.net/qq_35635374/article/details/124539162)

### 文章目录

- [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/124539162#_0)
- [前言](https://blog.csdn.net/qq_35635374/article/details/124539162#_13)
- [一、【地图的本质理解】](https://blog.csdn.net/qq_35635374/article/details/124539162#_24)
- [二、高精度地图生产构建步骤](https://blog.csdn.net/qq_35635374/article/details/124539162#_35)
    - [1.第一步：SLAM构建点云地图--（通过定位与点云数据,使用NDT建立点云地图）](https://blog.csdn.net/qq_35635374/article/details/124539162#1SLAMNDT_36)
    - [（1）点云地图的建立原理](https://blog.csdn.net/qq_35635374/article/details/124539162#1_38)[（2）点云地图建立算法及步骤](https://blog.csdn.net/qq_35635374/article/details/124539162#2_45)[1）点云地图建立经典的SLAM算法](https://blog.csdn.net/qq_35635374/article/details/124539162#1SLAM_46)[2）ndt_mapping功能包解析【算法核心】](https://blog.csdn.net/qq_35635374/article/details/124539162#2ndt_mapping_54)[3）DNT建立的点云地图的调参过程](https://blog.csdn.net/qq_35635374/article/details/124539162#3DNT_62)[4）保存pcd的点云地图](https://blog.csdn.net/qq_35635374/article/details/124539162#4pcd_67)[（3）点云特征匹配--获取位姿变化率](https://blog.csdn.net/qq_35635374/article/details/124539162#3_71)
    - [2.第二步：vector_map语义地图--（通过点云地图，标注制作语义信息，构建语义地图）](https://blog.csdn.net/qq_35635374/article/details/124539162#2vector_map_78)
    - [（1）矢量地图的定义](https://blog.csdn.net/qq_35635374/article/details/124539162#1_80)[（2）矢量地图绘制（语义信息标注autoware tools的工具使用）](https://blog.csdn.net/qq_35635374/article/details/124539162#2autoware_tools_86)[1）语义地图的图例](https://blog.csdn.net/qq_35635374/article/details/124539162#1_87)[2）矢量地图的标注与制作工具](https://blog.csdn.net/qq_35635374/article/details/124539162#2_93)[3）autoware_tools步骤](https://blog.csdn.net/qq_35635374/article/details/124539162#3autoware_tools_100)[（3）矢量地图的调用](https://blog.csdn.net/qq_35635374/article/details/124539162#3_119)[（4）矢量地图各个元素的识别](https://blog.csdn.net/qq_35635374/article/details/124539162#4_123)[具体绘制矢量地图的方法参考文档](https://blog.csdn.net/qq_35635374/article/details/124539162#_129)
    - [3.第三步：高精度地图=点云地图+矢量地图](https://blog.csdn.net/qq_35635374/article/details/124539162#3_134)
    - [1、高精度地图的定义](https://blog.csdn.net/qq_35635374/article/details/124539162#1_136)[2、高精度地图的生产流程](https://blog.csdn.net/qq_35635374/article/details/124539162#2_141)
    - [4.拓展：视觉方案：实时视觉地图](https://blog.csdn.net/qq_35635374/article/details/124539162#4_150)
- [三、map_file功能包地图解析步骤](https://blog.csdn.net/qq_35635374/article/details/124539162#map_file_155)
- [四、地图相关的总结](https://blog.csdn.net/qq_35635374/article/details/124539162#_166)
    - [（1）常用的地图的类型](https://blog.csdn.net/qq_35635374/article/details/124539162#1_167)
    - [（2）规划用到的地图形式](https://blog.csdn.net/qq_35635374/article/details/124539162#2_194)
    - [（3）简单地图的制作及调用](https://blog.csdn.net/qq_35635374/article/details/124539162#3_204)
    - [（1）栅格地图的制作](https://blog.csdn.net/qq_35635374/article/details/124539162#1_208)[（2）地图用map server （加载自己画的图）](https://blog.csdn.net/qq_35635374/article/details/124539162#2map_server__214)
    - [（4）栅格地图处理](https://blog.csdn.net/qq_35635374/article/details/124539162#4_216)
    - [（5）地图的作用](https://blog.csdn.net/qq_35635374/article/details/124539162#5_220)

---

## 前言

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！

本文先对**【autoware高精度地图模块】**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章

---

提示：以下是本篇文章正文内容

## 一、【地图的本质理解】

**地图就是一种数据结构**，地图格式就是用不同的数据类型和数据结构把地图信息存储起来，所以解析地图必须由标准的地图格式（如把csv转换成ros_topic）

高精度地图是其他模块的支撑（提供先验信息）

![[post-images/cb2eb0753c744fe8bf8d99adf26826a5.png]]

.

.

## 二、高精度地图生产构建步骤

### 1.第一步：[SLAM](https://so.csdn.net/so/search?q=SLAM&spm=1001.2101.3001.7020)构建点云地图–（通过定位与点云数据,使用NDT建立点云地图）

### （1）[点云](https://so.csdn.net/so/search?q=%E7%82%B9%E4%BA%91&spm=1001.2101.3001.7020)地图的建立原理

（1）【[点云数据](https://so.csdn.net/so/search?q=%E7%82%B9%E4%BA%91%E6%95%B0%E6%8D%AE&spm=1001.2101.3001.7020)的采集与坐标转换】激光雷达采集输入点云数据，激光雷达的点云数据是基于激光雷达坐标系的，因为激光雷达传感器与车辆是刚性连接的，故可以通过TF坐标变换转换成base_link坐标系下。

（2）【定位的获取】一般把车辆的起始点作为地图的原点，通过定位模块（中的绝对GPS/rtk等绝对型定位方式）获取准确的车辆位姿变化

（3）【全局点云数据与定位数据匹配，建立全局点云地图】在车辆运动过程中，将同一时间戳下的点云数据与定位数据进行匹配，得到一张局部点云图，以此往复，逐渐构建出一张庞大的点云地图

### （2）点云地图建立算法及步骤

### 1）点云地图建立经典的SLAM算法

![[post-images/620b8bf91cc84623a298d87361d692b4.png]]

DNT不带回环检测，DNT中等地图整体效果还不错，大地图还是用load方案好

其他方案关注我地图专栏的博客，转战一下~

.

.

### 2）ndt_mapping功能包解析【算法核心】

输入输出：输入激光雷达的点云数据、imu、odom，输出pcd点云地图

![[post-images/31ce72552c1d4ad899be96af76196afe.png]]

![[post-images/7223762003fd4c28aca9566a9eac4d41.png]]

.

.

### 3）DNT建立的点云地图的调参过程

中等地图还是可以的，通过调整建图分辨率可以得到不同稠密度的点云地图，通过launch文件的参数服务器可以不编译进行调试参数

![[post-images/a910cc5ec08343ebb6e969556b1356eb.png]]

.

### 4）保存pcd的点云地图

![[post-images/5af864560bd7486697ba833cf06dc1a0.png]]

.

.

### （3）点云特征匹配–获取位姿变化率

对于slam算法而言，位姿变化是通过点云特征匹配（ICP）并优化后得到的

![[post-images/44e476595cb74ea5bea8fb7f2f5aa3d0.png]]

![[post-images/a71d286e650241d495aaf4939f5cdcf3.png]]

.

.

### 2.第二步：vector_map语义地图–（通过点云地图，标注制作语义信息，构建语义地图）

### （1）矢量地图的定义

语义地图实际上是通过标定提供了特定场景的先验信息，语义地图元素的识别，如斑马线，红绿灯、停止线，语义地图元素的识别不属于感知的范畴，仅仅是先验语义地图的解析【.csv文件的解析而已】

![[post-images/0fb4e6fb610949a481c40d8236c2bf5e 2.png|0fb4e6fb610949a481c40d8236c2bf5e 2.png]]

### （2）矢量地图绘制（语义信息标注autoware tools的工具使用）

### 1）语义地图的图例

![[post-images/bb7c92c1e4c3429a949f527208b49593.png]]

（在点云地图的基础上）

（网页版比较好，需要win系统和好的网络环境）

（标注的信息最终会形成各种类型的.csv文件）

.

### 2）矢量地图的标注与制作工具

```Plain
（1）autowaer tools
（2）unity插件版本
（3）VTD
（4）roadrunner


1
2
3
4
```

.

### 3）autoware_tools步骤

1）注册、登录

![[post-images/ae83e63e1f3b44d6872f1fa4af6627b7.png]]

2）导入PCD文件点云地图、导入全局路径（可选）、导入之前已经绘制好的csv文件（在这个基础上进行补充：红绿灯、车道线、杆元素、斑马线、可通行区域等等）

![[post-images/03b8bf4bd5984540b089c22c76636600 2.png|03b8bf4bd5984540b089c22c76636600 2.png]]

3）根据点云形状，使用标注的工具人工绘制普通元素类型的工具

![[post-images/57a5c8e820474fe3a84a64ea1bc88144.png]]

包括road、road shape、road surface、road side 、structure、waypoints

4）据点云形状，人工绘制自定义元素，可通过改变普通元素的参数实现

![[post-images/3651242afe384924959ca174efcda915.png]]

Lane车道线可以先画一大条，然后自动分割成多段小的轨迹

5）人工语义地图的保存

![[post-images/6afbe04ad77a4fdcb7d33ff461a99a7e.png]]

保存为一个压缩包

.

### （3）矢量地图的调用

在map_file功能包，通过vector_map_loader这个节点实现的，本质上就是解析各类.csv文件，csv文件也是有相对应的关联的

.

### （4）矢量地图各个元素的识别

vector_map就是一个先验的信息，每个元素的link_id在绘制vector_map的时候确定下来了，autoware通过link_id编号识别vector_map的各个道路元素，如，在vector_map地图检测到红灯元素的link_id就会发送一个红灯的话题，让车辆行驶到红灯前停止下来！vector_map是用话题来触发的。车辆在行进过程中，link_id不断自增，同时不断删除走过的link_id

link_id的机制，vector_map会把不相关的目标进行过滤掉，减少工作量。在vector_map的车道内才会对目标进行预测，在车道外似乎不会进行目标检测与预测。

.

### 具体绘制矢量地图的方法参考文档

![[post-images/52f0b85880c14b4f9ba0d027dfd9f288.png]]

.

.

### 3.第三步：高精度地图=点云地图+矢量地图

### 1、高精度地图的定义

![[post-images/d948c1ecf25b46c481ca7a0985980362.png]]

高精度地图可以理解成为一个数据集，以一定的数据类型的结构体构成的数据集

### 2、高精度地图的生产流程

![[post-images/16f791c4390b420f9cb02ddf05386457.png]]

![[post-images/bd869b2f3d5b4e2d9bd2e985bfb460f6.png]]

（1）数据融合是传感器融合的范畴

（2）点云地图建立是SLAM的范畴

（3）下采样去噪点和语义信息标注就是一个无脑贴标签的操作

.

.

### 4.拓展：视觉方案：实时视觉地图

用于视觉融合匹配做视觉里程计VIO，或者对先验的目标进行匹配

.

.

## 三、map_file功能包地图解析步骤

地图模块中的map_file功能包解析（autoware的map_file功能包相当于move base的map_server功能包）

![[post-images/397b8a2868f24da78ee9218f8edd3ec2.png]]

(1)Points_map_loader节点是用来解析点云地图pcd的点云数据的，以当前定位数据为基准进行输出topic

(2)Vector_map_loader节点是用来解析人工绘制的.csv文件对应的道路元素的，以当前定位数据为基准进行输出topic

![[post-images/34fe339c2b6d4e47bbb58a9c53ba8b5d.png]]

## 四、地图相关的总结

### （1）常用的地图的类型

（1）代价栅格地图grid_map

```Plain
机器人的路径搜索和后端优化
查询某些点的占据情况


1
2
```

（2）点云地图

```Plain
感知、定位模块常用


1
```

（3）矢量地图

```Plain
决策、规划的全局路径


1
```

（4）ESDF的欧式距离场地图

```Plain
fast_planner的后端轨迹优化


1
```

（5）人工势场图

```Plain
人工势场算法中提供引力和斥力


1
```

（6）八叉树地图

```Plain
无人机三维规划


1
```

.

.

### （2）规划用到的地图形式

1、Lexicographer_planner有从点云地图转换成栅格地图的实现

2、a*类算法用到栅格地图

3、纯跟踪算法用到语义地图

4、fast_planner用到esdf欧式距离场地图

地图格式之间是可以相互转换的

.

.

### （3）简单地图的制作及调用

【建图的方式是多种多样的，所以路径规划的方式也可以是多种多样的】地图的建立不仅仅可以通过二维激光雷达建立一个二维栅格地图，通过三维激光雷达建立一个三维体素（八叉树）地图，还可以通过单目灰度相机建立一张灰度图（灰度图就是一张二维的栅格地图，一个栅格对应的就是一个像素），也可以通过深度相机获得局部的点云地图，建立局部的三维体素地图

### （1）栅格地图的制作

（1）用二维的SLAM建图算法

（2）用visual画出来生成png文件

（3）用matepad的云记绘制并导出png文件

.

### （2）地图用map server （加载自己画的图）

.

### （4）栅格地图处理

地图膨胀，原始占据地图（原始占据地图可以自己画），这样配合规划做

.

### （5）地图的作用

规划中，一般前端路径探索会用到地图，后端的障碍物远离轨迹优化也会用到，甚至基于曲线的路径探索，路径拟合，路径优化也是要用到地图的提供的位置点采样，查看目标点是否占据，规划出来的轨迹有没有发生碰撞等等

---

文章知识点与官方知识档案匹配，可进一步学习相关知识

[OpenCV技能树首页概览](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)23318 人正在系统学习中

显示推荐内容
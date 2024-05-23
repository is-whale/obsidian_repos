---
Created: 2023-12-18T14:16
Updated: 2023-12-18T14:16
URL: https://blog.csdn.net/qq_35635374/article/details/121859664
---
# **【无人驾驶autoware 项目实战】传感器数据获取、数据采集方案**

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121859664#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121859664#_13)
- [ ] [一、获取传感器点云（Point cloud）数据](https://blog.csdn.net/qq_35635374/article/details/121859664#Point_cloud_24)
- [ ] [二、获取图像（image）数据](https://blog.csdn.net/qq_35635374/article/details/121859664#image_34)
- [ ] [三、获取惯导系统（IMU）数据](https://blog.csdn.net/qq_35635374/article/details/121859664#IMU_41)
- [ ] [四、获取定位系统（GNSS）数据](https://blog.csdn.net/qq_35635374/article/details/121859664#GNSS_47)
- [ ] [五、获取全局三维点云地图数据、ADAS矢量语义地图数据](https://blog.csdn.net/qq_35635374/article/details/121859664#ADAS_56)
- [ ] [六、数据采集的方案示例](https://blog.csdn.net/qq_35635374/article/details/121859664#_66)
- [ ]
    - [ ] [（1）低成本移动激光测量进行数据采集的方案（适合自己做小型实验）](https://blog.csdn.net/qq_35635374/article/details/121859664#1_67)
    - [ ] [（2）一体化高精度地图SLAM的解决方案（适合公司运维）](https://blog.csdn.net/qq_35635374/article/details/121859664#2SLAM_72)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**【无人驾驶autoware 项目实战】传感器数据获取、数据采集方案**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

## **一、获取传感器**[**点云**](https://so.csdn.net/so/search?q=%E7%82%B9%E4%BA%91&spm=1001.2101.3001.7020)**（Point cloud）数据**

直接通过传感器驱动获取话题数据即可

Velodyne激光雷达传感器、Ibeo激光雷达传感器、Hokuyo激光雷达传感器、【防盗标记–盒子君hzj】毫米波雷达和惯性测量装置通常是自动飞行器的首选

摄像机和激光雷达传感器通常在10到100赫兹的频率下运行，每个任务自动驾驶必须在一定条件下进行时间限制

点云库（PCL）是主要用于管理激光雷达扫描和三维地图数据

.

.

## **二、获取图像（image）数据**

直接通过传感器驱动获取话题数据即可  
  
全方位覆盖360度视野，用来检测运动物体，用来检测运动物体识别红绿灯  
  
.  
  
.

## **三、获取惯导系统（IMU）数据**

直接通过传感器驱动获取话题数据即可  
  
.  
  
.

## **四、获取定位系统（GNSS）数据**

直接通过传感器驱动获取话题数据即可  
  
定位系统gnss传感器接收来自卫星的全球定位信息，通常与陀螺IMU传感器和里程表相结合，以确定位置信息。  
  
定位系统（GNSS）数据在定位模块，有一个功能包gnss_localizer  
  
fix2tfpose包含于gnss_localizer功能包中，通过转换GNSS接收器发来的NEMA消息得到位置消息，后面还要自己实现还有就是全局和局部坐标系的转换。转换GNSS接收器发来的NEMA消息到位置信息(x,y,z,roll,pitch,yaw)。结果可以被单独使用为车辆当前位置，也可以作为lidar_localizar的初始参考位置  
  
.  
  
.

## **五、获取全局三维点云地图数据、ADAS矢量语义地图数据**

全局三维点云地图数据直接读取.pcd文件（内存真的很大），挥着通过map_server功能包读取二维地图

ADAS矢量语义地图直接读取.csv文件 根据高精度点云地图，图像地图，人工绘制的语义地图（一般由公司专门去做的，相当于给定了.csv文件的航线lane、交通灯等等更多的静态地图信息） .csv文件可以看看我另外的博客 [【地图】地图矢量化–生成矢量化地图的方法](https://blog.csdn.net/qq_35635374/article/details/120920983)

.

.

## **六、数据采集的方案示例**

### **（1）低成本移动激光测量进行数据采集的方案（适合自己做小型实验）**

在小车等移动载体上采集得到激光雷达高精度点云数据、甚至是相机数据、GPS/IMU航迹里程计数据 . .

![[watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

### **（2）一体化高精度地图SLAM的解决方案（适合公司运维）**

. .

![[154121c935264eb0bd65bbd22524338c.png]]

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
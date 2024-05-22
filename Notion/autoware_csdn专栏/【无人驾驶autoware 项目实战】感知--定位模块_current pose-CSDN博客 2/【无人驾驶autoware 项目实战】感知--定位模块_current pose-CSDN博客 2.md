---
Created: 2023-12-18T14:15
Updated: 2023-12-18T14:15
URL: https://blog.csdn.net/qq_35635374/article/details/121859820
---
# **【无人驾驶autoware 项目实战】感知--定位模块**

![[images/original 17.png|original 17.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newCurrentTime2 13.png|newCurrentTime2 13.png]]

于 2021-12-10 16:42:12 发布

![[images/articleReadEyes2 17.png|articleReadEyes2 17.png]]

阅读量1.2k

收藏  
  
  
7  

![[images/tobarCollect2 17.png|tobarCollect2 17.png]]

![[images/newHeart2023Black 17.png|newHeart2023Black 17.png]]

点赞数  
4  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 16.|resize2Cm_fixed2Ch_2242Cw_224 16.]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 16.png|studyVipIcon 16.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121859820#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121859820#_13)
- [ ] [一、功能](https://blog.csdn.net/qq_35635374/article/details/121859820#_28)
- [ ] [二、输入](https://blog.csdn.net/qq_35635374/article/details/121859820#_39)
- [ ]
    - [ ] [【pointcloud data】](https://blog.csdn.net/qq_35635374/article/details/121859820#pointcloud_data_40)
    - [ ] [【pointcloud map】](https://blog.csdn.net/qq_35635374/article/details/121859820#pointcloud_map_45)
    - [ ] [【GNSS pose】](https://blog.csdn.net/qq_35635374/article/details/121859820#GNSS_pose_49)
    - [ ] [【IMU】](https://blog.csdn.net/qq_35635374/article/details/121859820#IMU_53)
- [ ] [三、输出](https://blog.csdn.net/qq_35635374/article/details/121859820#_58)
- [ ]
    - [ ] [【current pose】](https://blog.csdn.net/qq_35635374/article/details/121859820#current_pose_60)
    - [ ] [【current velocity】](https://blog.csdn.net/qq_35635374/article/details/121859820#current_velocity_63)
- [ ] [四、算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121859820#_71)
- [ ]
    - [ ] [1.【current velocity】](https://blog.csdn.net/qq_35635374/article/details/121859820#1current_velocity_72)
    - [ ] [2.【lidar_localizer(ndt_matching)】](https://blog.csdn.net/qq_35635374/article/details/121859820#2lidar_localizerndt_matching_81)
    - [ ] [3.【gnss_localizer（fix2tfpose）】](https://blog.csdn.net/qq_35635374/article/details/121859820#3gnss_localizerfix2tfpose_87)
    - [ ] [4.【dead_reckoner】](https://blog.csdn.net/qq_35635374/article/details/121859820#4dead_reckoner_93)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121859820#_107)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121859820#_115)

---

## **前言**

  
  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**【无人驾驶autoware 项目实战】感知–定位模块**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 13.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 13.]]

## **一、功能**

定位模块使用[SLAM](https://so.csdn.net/so/search?q=SLAM&spm=1001.2101.3001.7020)算法、3D map(高精度地图)服务、NDT来实现，使用从CAN消息和GNSS/IMU传感器获取的里程计信息，通过Kalman滤波算法对定位结果进行补充

Autoware主要使用NDT算法进行局部化。这是因为无损检测的计算成本算法不受地图大小的支配，从而使高清晰度和高分辨率3D的部署大比例尺地图

Autoware还支持其他本地化和映射算法，如迭代最近点（ICP）算法。这允许Autoware用户为其应用程序选择最适合的算法

.

.

## **二、输入**

### **【pointcloud data】**

激光雷达的点云数据（经过PCL下采样滤波的点云处理后的数据  
  
.

### **【pointcloud map】**

SLAM建立的三维点云地图  
  
.

### **【GNSS pose】**

GNSS绝对型定位信息  
  
.

### **【IMU】**

惯性导航数据  
  
.  
  
.

## **三、输出**

### **【current pose】**

无人车的当前位姿  
  
.

### **【current velocity】**

无人车的当前速度  
  
.  
  
.

## **四、算法流程实现**

### **1.【current velocity】**

车辆的当前速度current velocity是根据定位信息速度localizer velocity 和车底盘里程计CAN velocity融合得到的  
  
立个flag…效果、算法原理、源码我在整理一次再更~  
  
.  
  
.

### **2.【lidar_localizer(ndt_matching)】**

根据预先构建的点云地图和实时激光雷达LIDAR点云数据，autoware推荐使用正态分布变换(NDT)算法来匹配激光雷达当前帧和3D map，计算车辆当在全局坐标的当前位置(x,y,z,roll,pitch,yaw) 立个flag…效果、算法原理、源码我在整理一次再更~ . .

![[images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 6.|watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 6.]]

### **3.【gnss_localizer（fix2tfpose）】**

fix2tfpose包含于gnss_localizer功能包中，通过转换GNSS接收器发来的NEMA消息得到位置消息，后面还要自己实现还有就是全局和局部坐标系的转换。转换GNSS接收器发来的NEMA消息到位置信息(x,y,z,roll,pitch,yaw)。结果可以被单独使用为车辆当前位置，也可以作为lidar_localizar的初始参考位置  
  
立个flag…效果、算法原理、源码我在整理一次再更~  
  
.  
  
.

### **4.【dead_reckoner】**

用于优化定位的，使用当前车辆速度（current velocity）、IMU传感器预测车辆的下一帧位置增量delte pose，可以用来对lidar_localizar和gnss_localizar的结果进行插值  
  
立个flag…效果、算法原理、源码我在整理一次再更~  
  
.  
  
.

## **总结**

## **参考资料**

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
---
Created: 2023-12-18T14:14
Updated: 2023-12-18T14:14
URL: https://blog.csdn.net/qq_35635374/article/details/121859878
---
# **【无人驾驶autoware 项目实战】感知-目标检测模块**

![[images/original 2.png|original 2.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newCurrentTime2 2.png|newCurrentTime2 2.png]]

于 2021-12-10 16:43:32 发布

![[images/articleReadEyes2 2.png|articleReadEyes2 2.png]]

阅读量1.8k

收藏  
  
  
7  

![[images/tobarCollect2 2.png|tobarCollect2 2.png]]

![[images/newHeart2023Black 2.png|newHeart2023Black 2.png]]

点赞数  
4  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==目标检测==](https://so.csdn.net/so/search/s.do?q=%E7%9B%AE%E6%A0%87%E6%A3%80%E6%B5%8B&t=all&o=vip&s=&l=&f=&viparticle=)[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 2.|resize2Cm_fixed2Ch_2242Cw_224 2.]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 2.png|studyVipIcon 2.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121859878#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121859878#_13)
- [ ] [一、功能](https://blog.csdn.net/qq_35635374/article/details/121859878#_25)
- [ ] [二、输入](https://blog.csdn.net/qq_35635374/article/details/121859878#_35)
- [ ]
    - [ ] [【camera image】](https://blog.csdn.net/qq_35635374/article/details/121859878#camera_image_36)
    - [ ] [【pointcloud data】](https://blog.csdn.net/qq_35635374/article/details/121859878#pointcloud_data_40)
- [ ] [三、输出](https://blog.csdn.net/qq_35635374/article/details/121859878#_46)
- [ ]
    - [ ] [【image objects】](https://blog.csdn.net/qq_35635374/article/details/121859878#image_objects_47)
    - [ ] [【points objects】](https://blog.csdn.net/qq_35635374/article/details/121859878#points_objects_50)
    - [ ] [【objects class】](https://blog.csdn.net/qq_35635374/article/details/121859878#objects_class_53)
- [ ] [四、算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121859878#_61)
- [ ]
    - [ ] [【image detector】](https://blog.csdn.net/qq_35635374/article/details/121859878#image_detector_62)
    - [ ] [【lidar_detector（clustering）】](https://blog.csdn.net/qq_35635374/article/details/121859878#lidar_detectorclustering_70)
    - [ ] [【fusion_tools】](https://blog.csdn.net/qq_35635374/article/details/121859878#fusion_tools_79)
    - [ ] [【objects fusion】](https://blog.csdn.net/qq_35635374/article/details/121859878#objects_fusion_85)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121859878#_103)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121859878#_111)

---

## **前言**

  
  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**【无人驾驶autoware 项目实战】感知-目标检测模块**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

## **一、功能**

自动驾驶车辆必须检测周围物体，如其他车辆、行人和行人交通信号灯。有了这些信息，我们能遵守交通规则，避免事故发生。检测模块使用摄像头和激光雷达，结合传感器融合算法和深度学习网络进行目标检测。目标识别与检测，基于先进的YOLOv3检测模型，实时地检测车辆前方环境以及障碍，实现自主停障和在监视屏上显示当前行驶的状态  
  
探测物体，比如车辆，行人和交通信号灯，以避免碰撞和违反交通规则。【防盗标记–盒子君hzj】我们专注于在移动物体（车辆和行人）上，虽然我们的平台也能识别交通信号灯。我们使用可变形零件模型（DPM）算法进行检测车辆和行人。  
  
CUDA是NVIDIA开发的一个编程框架，用于通用计算在GPU  
  
.  
  
.

## **二、输入**

### **【camera image】**

相机图像  
  
.

### **【pointcloud data】**

激光雷达的点云数据（经过PCL下采样滤波的点云处理后的数据  
  
.  
  
.

## **三、输出**

### **【image objects】**

各种图像目标检测类别  
  
.

### **【points objects】**

各种点云目标检测类别  
  
.

### **【objects class】**

各种融合的目标类别  
  
.  
  
.

## **四、算法流程实现**

### **【image detector】**

图像的目标检测：输入相机图像camera image，提供基于图像的目标检测。使用YOLO、SSD、R-CNN等的算法，识别出来图像各种目标类别image objects，可以进行多类别(汽车，行人等)实时目标检测。识别交通标识、识别动态人车信息  
  
立个flag…效果、算法原理、源码我在整理一次再更~  
  
.

### **【lidar_detector（clustering）】**

点云聚类（三维电锯检测的一种手段）：输入实时激光雷达点云数据，经过聚类，输出点云各种目标类别points objects。从激光雷达单帧扫描读取点云信息，提供基于激光雷达的目标检测。【防盗标记–盒子君hzj】主要使用欧几里德聚类算法，从地面以上的点云得到聚类结果。除此之外，可以使用基于卷积神经网路的算法进行分类，包括VoxelNet,LMNet.  
  
点云数据配准技术:点云畸变的纠偏  
  
点云障碍物检测技术:从激光雷达中的点云数据中识别检测出行人、车辆障碍物等等  
  
立个flag…效果、算法原理、源码我在整理一次再更~  
  
.

### **【fusion_tools】**

将lidar_detector和image_detector的检测结果进行融合，image_detector 的识别类别被添加到lidar_detector的聚类结果上。  
  
立个flag…效果、算法原理、源码我在整理一次再更~  
  
.

### **【objects fusion】**

图像、点云目标融合：输入激光雷达的单帧扫描点云和摄像头的图片信息，进行在3D空间的更准确的目标检测。激光雷达的位置和摄像头的位置需要提前进行联合标定，现在主要是基于MV3D算法来实现。经过图像、点云目标融合，得到各种融合的目标类别  
  
立个flag…效果、算法原理、源码我在整理一次再更~  
  
.  
  
.

## **总结**

## **参考资料**

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[Python入门技能树](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)_379603_ _人正在系统学习中_

 _显示推荐内容_
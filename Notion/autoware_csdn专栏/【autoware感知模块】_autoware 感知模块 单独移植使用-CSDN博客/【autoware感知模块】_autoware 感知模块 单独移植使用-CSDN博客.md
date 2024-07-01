---
Created: 2023-12-18T14:11
Updated: 2023-12-18T14:11
URL: https://blog.csdn.net/qq_35635374/article/details/124538712
---
# **【autoware感知模块】**

![[images/original 3.png|original 3.png]]

![[identityVip.png]]

VIP文章

[盒子君~](https://blog.csdn.net/qq_35635374)

![[newUpTime2.png]]

已于 2022-05-02 12:16:26 修改

![[images/articleReadEyes2 3.png|articleReadEyes2 3.png]]

阅读量823

收藏  
  
  
6  

![[images/tobarCollect2 3.png|tobarCollect2 3.png]]

![[images/newHeart2023Black 3.png|newHeart2023Black 3.png]]

点赞数

分类专栏：[==# 感知perception==](https://blog.csdn.net/qq_35635374/category_11464719.html)[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)[==聚类==](https://so.csdn.net/so/search/s.do?q=%E8%81%9A%E7%B1%BB&t=all&o=vip&s=&l=&f=&viparticle=)

版权

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/124538712#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/124538712#_13)
- [ ] [一、感知的四大基础任务-检测、分割、跟踪、预测](https://blog.csdn.net/qq_35635374/article/details/124538712#_24)
- [ ] [二、常见感知方法的概述](https://blog.csdn.net/qq_35635374/article/details/124538712#_37)
- [ ]
    - [ ] [1.自动驾驶中存在的感知任务（检测、跟踪、预测）](https://blog.csdn.net/qq_35635374/article/details/124538712#1_40)
    - [ ] [2.完成感知任务需要的传感器](https://blog.csdn.net/qq_35635374/article/details/124538712#2_44)
    - [ ] [3.感知的数据输入（激光雷达lidar和摄像头方案）](https://blog.csdn.net/qq_35635374/article/details/124538712#3lidar_48)
    - [ ]
        - [ ] [点云数据信息](https://blog.csdn.net/qq_35635374/article/details/124538712#_51)
        - [ ] [图像数据](https://blog.csdn.net/qq_35635374/article/details/124538712#_58)
- [ ] [三、目标点云聚类、识别、追踪](https://blog.csdn.net/qq_35635374/article/details/124538712#_64)
- [ ]
    - [ ] [1.点云识别第一步【Euclidean_cluster点云的欧式聚类算法模块】](https://blog.csdn.net/qq_35635374/article/details/124538712#1Euclidean_cluster_65)
    - [ ]
        - [ ] [（1）模块介绍](https://blog.csdn.net/qq_35635374/article/details/124538712#1_67)
        - [ ] [（2）欧式聚类的原理步骤](https://blog.csdn.net/qq_35635374/article/details/124538712#2_72)
        - [ ] [（3）关键函数及源码解读](https://blog.csdn.net/qq_35635374/article/details/124538712#3_80)
        - [ ] [（4）算法效果](https://blog.csdn.net/qq_35635374/article/details/124538712#4_85)
    - [ ] [2.点云识别第二步【roi_objects_filter模块源码解析及实战--道路可通行区域】](https://blog.csdn.net/qq_35635374/article/details/124538712#2roi_objects_filter_96)
    - [ ]
        - [ ] [（1）模块介绍](https://blog.csdn.net/qq_35635374/article/details/124538712#1_98)
        - [ ] [（2）关键函数fun](https://blog.csdn.net/qq_35635374/article/details/124538712#2fun_108)
        - [ ]
            - [ ] [1）语义地图转栅格地图](https://blog.csdn.net/qq_35635374/article/details/124538712#1_109)
            - [ ] [2）基于栅格地图对目标object进行过滤a](https://blog.csdn.net/qq_35635374/article/details/124538712#2objecta_111)
        - [ ] [（3）算法效果](https://blog.csdn.net/qq_35635374/article/details/124538712#3_116)
        - [ ]
            - [ ] [1）通过语义地图划定一个可通行区域](https://blog.csdn.net/qq_35635374/article/details/124538712#1_117)
            - [ ] [2）可通行区域转换成栅格地图grid_map](https://blog.csdn.net/qq_35635374/article/details/124538712#2grid_map_122)
            - [ ] [3）遍历每个对象，过滤掉不在roi的对象](https://blog.csdn.net/qq_35635374/article/details/124538712#3roi_127)
    - [ ] [3.点云识别第三步【lidar_kf_contour_track模块源码解析】](https://blog.csdn.net/qq_35635374/article/details/124538712#3lidar_kf_contour_track_132)
    - [ ]
        - [ ] [（0）基础知识](https://blog.csdn.net/qq_35635374/article/details/124538712#0_134)
        - [ ] [（1）模块介绍](https://blog.csdn.net/qq_35635374/article/details/124538712#1_142)
        - [ ] [（2）关键函数](https://blog.csdn.net/qq_35635374/article/details/124538712#2_152)
        - [ ]
            - [ ] [1、目标外接多边形的估计【shap边界估计算法模块】](https://blog.csdn.net/qq_35635374/article/details/124538712#1shap_153)
            - [ ] [2、目标的追踪](https://blog.csdn.net/qq_35635374/article/details/124538712#2_160)
        - [ ] [（3）算法效果](https://blog.csdn.net/qq_35635374/article/details/124538712#3_167)
- [ ] [四、图像识别、定位、追踪](https://blog.csdn.net/qq_35635374/article/details/124538712#_180)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**autoware感知模块**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

## **一、感知的四大基础任务-检测、分割、跟踪、预测**

预测模块（比较难，还没有确定落地的方案）  
  
  
  
.  
  
.

![[post-images/ba7823c7287446aab46d072054e2446c.png]]

![[post-images/abe39f4f68a9478ab9e16a0d6cfe4b4a.png]]

![[post-images/f8efac4e59dc476689d3ec710649914a.png]]

## **二、常见感知方法的概述**

![[post-images/dc4dbb37235c4e25908711a02a11f841.png]]

### **1.**[**自动驾驶**](https://so.csdn.net/so/search?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&spm=1001.2101.3001.7020)**中存在的感知任务（检测、跟踪、预测）**

![[post-images/f9337ae442c74ee39c674819ac62eb06.png]]

感知模块的功能是最广泛的，Autoware的感知部分还包括yolov3等的摄像头感知等功能！这里仅仅抽取了一些简单的功能进行讲解

### **2.完成感知任务需要的传感器**

![[post-images/4a6da2d31568405ba41f28dc467b2a90.png]]

uss是超声波雷达、

[lidar](https://so.csdn.net/so/search?q=lidar&spm=1001.2101.3001.7020)

是激光雷达、radar是毫米波雷达

### **3.感知的数据输入（**[**激光雷达**](https://so.csdn.net/so/search?q=%E6%BF%80%E5%85%89%E9%9B%B7%E8%BE%BE&spm=1001.2101.3001.7020)**lidar和摄像头方案）**

![[post-images/0509b5c7c1a54fceb27d37e393611184.png]]

### **点云数据信息**

**点云信息一般由激光雷达传感器提供**，激光雷达传感器输入原始的点云信息，点云信息要进行滤波、下采样、切割等PCL库API能解决的步骤，然后进一步进行欧式聚类，最后通过深度学习的方法对目标进行分类。【点云方向】  
  
对于激光雷达点云的使用。这里和SLAM地图模块不一样的是，SLAM地图模块也要进行点云信息要进行滤波、下采样步骤，然后生成一张全局的点云地图PCD同时提供定位信息【点云方向】

### **图像数据**

- *图像信息一般由相机传感器提供。**相机传感器输入原始的图像，原始图像一般要经过opencv库的预处理，包括下采样与滤波，然后用于目标检测、跟踪与预测【视觉方向–传统的方法与机器学习】
    
    .
    
    .
    

## **三、目标点云聚类、识别、追踪**

### **1.点云识别第一步【Euclidean_cluster点云的欧式聚类算法模块】**

### **（1）模块介绍**

lidar_euclidean_clustering模块主要负责对点云进行分割聚类，生成聚类cluster和object对象的几何特征被其他模块调用

![[post-images/6f265ade1bd74567a48c706f9c4793a8.png]]

### **（2）欧式聚类的原理步骤**

使用递归和KD_tree的方法，最终把杂乱无章的点云有序的存入集合Q中  
  
  
  
KD_tree的数据结构介绍–用于检索查找元素的

![[post-images/a92a0114ac9e4709adeb5c674735080d.png]]

![[post-images/1232303320c845d7a3a66f60e3ca6411.png]]

### **（3）关键函数及源码解读**

![[post-images/566bd8b334fe45b7a367531c508580dd.png]]

### **（4）算法效果**

第一步：实现的点云类聚的效果  
  
【以类似的点云聚合到一起，是点云处理的第一步】  
  
  
  
.  
  
.

![[post-images/9e682a943ba042fd8603177b9b229f07.png]]

![[post-images/050878b2d4784181ae7ca5e35b477500.png]]

### **2.点云识别第二步【roi_objects_filter模块源码解析及实战–道路可通行区域】**

### **（1）模块介绍**

roi是车辆的可通行区域，其他感知检测模块会识别出所有目标（包括树，房子等）的几何形状大小和类别，roi_objects_filter模块会根据语义地图，分割出仅仅在车辆的可通行区域roi区域的目标（大多数是行人、车辆和障碍物），用于决策规划【在可通行区域内的目标才是有效的目标】  
  
  
  
**算法步骤**  
  
1、把语义地图vector_map转换成为栅格地图grid_map，其中栅格地图grid_map是在roi范围内的。  
  
2、遍历感知模块检测出来的每个对象obiect，检测每个对象obiect是否在栅格地图grid_map中，若在则证明是一个有效的目标对象obiect  
  
.  
  
.

![[post-images/f46deee3589c4195951d13cf6d046052.png]]

### **（2）关键函数fun**

### **1）语义地图转栅格地图**

![[post-images/a15877f82a724f349141c7a1c05fe1fe.png]]

### **2）基于栅格地图对目标object进行过滤a**

![[post-images/fe876c8be43b4735be996fd37d6c9e11.png]]

### **（3）算法效果**

### **1）通过语义地图划定一个可通行区域**

![[post-images/33d9f8e2961e401da1494d61117a2e8e.png]]

### **2）可通行区域转换成栅格地图grid_map**

![[post-images/e14b4fa19aeb49798395a28cda767e6b.png]]

### **3）遍历每个对象，过滤掉不在roi的对象**

  
  
.  
  
.

![[post-images/550f4a6238c84f9695a5da360ddfd79a.png]]

### **3.点云识别第三步【lidar_kf_contour_track模块源码解析】**

### **（0）基础知识**

卡尔曼滤波器原理

![[post-images/b5f535436eaa49dfbd19d4778a68f7ae.png]]

具体的原理可以参考一下我下面这篇博客

[http://t.csdn.cn/DDcrC](http://t.csdn.cn/DDcrC)

.

.

### **（1）模块介绍**

lidar_kf_contour_track模块实现对特定对象的关联，并对特定对象进行追踪，输出最终的轨迹  
  
  
  
通过上述的点云处理，我们得到了点云簇，根据定位信息和点云簇，我们就可以实现对目标的追踪了。  
  
模块输入：定位信息和点云簇  
  
模块输出：要跟踪的目标（后续传递给规划模块，作为目标点）、目标的多边形轮廓

![[post-images/efa4c2aad3554b869db7b1a46a3c16fa.png]]

### **（2）关键函数**

### **1、目标外接多边形的估计【shap边界估计算法模块】**

估计外接多边形还可以通过opencv实现的，但是这里使用的是点云数据实现，所以要自己写。目标的几何外接多边形能比box提供更丰富的信息，视觉yolo仅仅能提供一个box矩形  
  
  
  
.  
  
.

![[post-images/07fca67392094cf499299da1189b21f6.png]]

### **2、目标的追踪**

步骤： 1、先进行对象object的匹配（类似于车牌识别这样） 2、输出对象object的坐标点及对象object走过的轨迹，使用规划对对象object进行跟踪 . .

![[post-images/025a60f55c45463c97781fc64b2da1ef.png]]

### **（3）算法效果**

1、第一步：对象object的匹配  
  
  
  
2、第二步：实现目标检测的roi_objects_filter，识别出目标的几何形状大小  
  
  
  
3、第三步：输出对象object走过的轨迹  
  
  
  
.  
  
.

![[post-images/b5615c2527be4ad0b63e664061eb81e3.png]]

![[post-images/42c925c285764060844a9b2a46e1d07a.png]]

![[post-images/bba75f81dce9490985576e04a80ac278.png]]

## **四、图像识别、定位、追踪**

yolov3等的摄像头感知等功能

参考我写的下面几篇入门 [https://blog.csdn.net/qq_35635374/article/details/121929980](https://blog.csdn.net/qq_35635374/article/details/121929980) [https://blog.csdn.net/qq_35635374/article/details/121983868](https://blog.csdn.net/qq_35635374/article/details/121983868) [https://blog.csdn.net/qq_35635374/article/details/121984039](https://blog.csdn.net/qq_35635374/article/details/121984039)

---

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
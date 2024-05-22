---
Created: 2023-12-18T14:15
Updated: 2023-12-18T14:15
URL: https://blog.csdn.net/qq_35635374/article/details/121859747
---
# **【无人驾驶autoware 项目实战】高精度点云地图建立、ADAS矢量语义地图建立**

![[images/original 6.png|original 6.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newCurrentTime2 5.png|newCurrentTime2 5.png]]

于 2021-12-10 16:42:28 发布

![[images/articleReadEyes2 6.png|articleReadEyes2 6.png]]

阅读量4.7k

收藏  
  
  
46  

![[images/tobarCollect2 6.png|tobarCollect2 6.png]]

![[images/newHeart2023Black 6.png|newHeart2023Black 6.png]]

点赞数  
6  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 5.|resize2Cm_fixed2Ch_2242Cw_224 5.]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 5.png|studyVipIcon 5.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121859747#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121859747#_13)
- [ ] [一、三维高精度点云地图建立](https://blog.csdn.net/qq_35635374/article/details/121859747#_25)
- [ ]
    - [ ] [1.功能](https://blog.csdn.net/qq_35635374/article/details/121859747#1_26)
    - [ ] [2.输入](https://blog.csdn.net/qq_35635374/article/details/121859747#2_29)
    - [ ] [3.输出](https://blog.csdn.net/qq_35635374/article/details/121859747#3_32)
    - [ ] [4.算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121859747#4_42)
- [ ] [二、ADAS矢量地图建立](https://blog.csdn.net/qq_35635374/article/details/121859747#ADAS_67)
- [ ]
    - [ ] [1.功能](https://blog.csdn.net/qq_35635374/article/details/121859747#1_68)
    - [ ] [2.工具及小型矢量地图建立方法](https://blog.csdn.net/qq_35635374/article/details/121859747#2_75)

---

## **前言**

  
  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**【无人驾驶autoware 项目实战】高精度点云地图建立、ADAS矢量语义地图建立**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

## **一、三维高精度点云地图建立**

### **1.功能**

通过在每次扫描时注册3D点云数据，创建并更新3D地图。这通常被称为SLAM，【防盗标记–盒子君hzj】基于实时定位与建图（SLAM）技术，把道路上的路标、障碍物等信息数字化，精确的描绘出来  
  
.

### **2.输入**

传感器点云数据（点云数据可以来自深度相机、激光雷达、毫米波雷达），使用SLAM内的定位或者输入外部的定位模块数据（外部定位数据可以是GPS/GNSS定位数据）  
  
.

### **3.输出**

3D 点云地图数据（3d map data）

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 3.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 3.]]

高精度地图的输出格式是怎样的？ [https://github.com/ApolloAuto/apollo/blob/master/modules/map/data/README.md](https://github.com/ApolloAuto/apollo/blob/master/modules/map/data/README.md) 参考README 所示，xml, bin, txt, lb1 都是不同的文件格式，适配不同的读取器，内容是一致的

.

### **4.算法流程实现**

使用三维激光雷达建图算法、视觉建图算法 具体可以参考我的地图mapping专栏，这里列几个相关我的博客 [【地图mapping】三维全局地图的开源方案及对比–NDT、LOAM、LIO-SAM、ALOAM、FLOAM、Lego_loam、SC-Lego-LOAM…](https://blog.csdn.net/qq_35635374/article/details/121002668)

[【地图mapping】视觉全局地图的开源方案及对比–rgbdslam、ORB_SLAM、RTAB SLAM](https://blog.csdn.net/qq_35635374/article/details/121003037)

[【地图mapping】（1）地图的理解及地图的类型介绍](https://blog.csdn.net/qq_35635374/article/details/120960481)

[【地图mapping】（2）地图SLAM构建基础理解](https://blog.csdn.net/qq_35635374/article/details/120978752)

[【地图mapping】（3）构建地图准备](https://blog.csdn.net/qq_35635374/article/details/120979417)

[【地图mapping】（4.1）构建特征点云地图–2D激光雷达地图构建介绍（简单综述）](https://blog.csdn.net/qq_35635374/article/details/120979524)

[【地图mapping】（4.2）构建特征点云地图–3D激光雷达地图构建介绍](https://blog.csdn.net/qq_35635374/article/details/120981832)

[【地图mapping 】（4.3）构建特征点云地图–视觉SLAM介绍](https://blog.csdn.net/qq_35635374/article/details/121000052)

[【地图mapping】（4.4）其他建图方法](https://blog.csdn.net/qq_35635374/article/details/121000838)

.

.

## **二、ADAS矢量地图建立**

### **1.功能**

根据高精度点云地图，图像地图，人工绘制的语义地图（一般由公司专门去做的，相当于给定了.csv文件的航线lane、交通灯等等更多的静态地图信息）  
  
协助校验环境位置信息，并对环境信息进行多方面的补充 ，包括路线route、车道线lane、路径点waypoints等等

![[images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2.|watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2.]]

### **2.工具及小型矢量地图建立方法**

一般都是找地图公司合作获取的，毕竟信息数据采集和优化、鼠标绘制地图还是很耗费人力物力的，像深度学习图像训练集标注一样。自己搞的话autoware比赛由相关的软件绘制

参考我另外一篇博客 [https://blog.csdn.net/qq_35635374/article/details/120920983](https://blog.csdn.net/qq_35635374/article/details/120920983)

---

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
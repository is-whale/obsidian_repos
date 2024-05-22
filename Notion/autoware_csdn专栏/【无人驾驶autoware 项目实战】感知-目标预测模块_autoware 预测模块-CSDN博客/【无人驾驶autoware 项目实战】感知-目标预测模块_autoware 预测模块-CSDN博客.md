---
Created: 2023-12-18T14:14
Updated: 2023-12-18T14:14
URL: https://blog.csdn.net/qq_35635374/article/details/121859976
---
# **【无人驾驶autoware 项目实战】感知-目标预测模块**

![[images/original 10.png|original 10.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newCurrentTime2 7.png|newCurrentTime2 7.png]]

于 2021-12-10 16:45:42 发布

![[images/articleReadEyes2 10.png|articleReadEyes2 10.png]]

阅读量703

收藏  
  
  
5  

![[images/tobarCollect2 10.png|tobarCollect2 10.png]]

![[images/newHeart2023Black 10.png|newHeart2023Black 10.png]]

点赞数  
3  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 9.|resize2Cm_fixed2Ch_2242Cw_224 9.]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 9.png|studyVipIcon 9.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121859976#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121859976#_13)
- [ ] [一、功能](https://blog.csdn.net/qq_35635374/article/details/121859976#_28)
- [ ] [二、输入](https://blog.csdn.net/qq_35635374/article/details/121859976#_38)
- [ ]
    - [ ] [【objects ID】](https://blog.csdn.net/qq_35635374/article/details/121859976#objects_ID_39)
    - [ ] [【ADAS 矢量地图】](https://blog.csdn.net/qq_35635374/article/details/121859976#ADAS__43)
- [ ] [三、输出](https://blog.csdn.net/qq_35635374/article/details/121859976#_48)
- [ ]
    - [ ] [【semantics map】](https://blog.csdn.net/qq_35635374/article/details/121859976#semantics_map_49)
    - [ ] [【potential map】](https://blog.csdn.net/qq_35635374/article/details/121859976#potential_map_53)
- [ ] [四、算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121859976#_58)
- [ ]
    - [ ] [【semantics mapper】](https://blog.csdn.net/qq_35635374/article/details/121859976#semantics_mapper_60)
    - [ ] [【potential mapper】](https://blog.csdn.net/qq_35635374/article/details/121859976#potential_mapper_64)
    - [ ] [【moving_predictor】](https://blog.csdn.net/qq_35635374/article/details/121859976#moving_predictor_68)
    - [ ] [【collision_predictor 】](https://blog.csdn.net/qq_35635374/article/details/121859976#collision_predictor__73)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121859976#_85)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121859976#_93)

---

## **前言**

  
  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**感知-目标预测模块**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 6.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 6.]]

## **一、功能**

自动驾驶汽车的安全是一个高度优先的问题。因此，感知模块必须计算出“自我”飞行器在三维空间中的准确位置映射，并识别周围场景中的对象作为交通信号灯的状态。预测模块使用**定位**和**检测的结果**来预测跟踪目标。通过**卡尔曼滤波算法**和**3D高精度地图**实现，基于**概率机器人技术**和**基于规则的系统**，部分还使用**深度神经网络**  
  
基于高精度模块、三维目标检测、追踪模块的信息，**预判**出附近车体的未来**轨迹**，使规划模块**规划**出更实时、安全的**路径**  
  
  
  
.  
  
.

![[images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 5.|watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 5.]]

## **二、输入**

### **【objects ID】**

要跟踪的目标类别ID  
  
.

### **【ADAS 矢量地图】**

根据高精度点云地图，图像地图，人工绘制的语义地图（一般由公司专门去做的，相当于给定了.csv文件的航线lane、交通灯等等更多的静态地图信息）  
  
.  
  
.

## **三、输出**

### **【semantics map】**

语义学地图  
  
.

### **【potential map】**

潜在预测动作地图  
  
.  
  
.

## **四、算法流程实现**

### **【semantics mapper】**

语义学地图生成，这个地图表明目标当前动作的含义，例如车辆打转向灯就表明车辆下一步即将想转向  
  
.

### **【potential mapper】**

潜在预测动作地图，这个地图表明了目标下一步动作的置信度  
  
.

### **【moving_predictor】**

使用目标跟踪的结果来预测临近物体的未来行动轨迹，例如汽车或者行人  
  
.

### **【collision_predictor 】**

使用moving_predictor的结果来进一步预测未来是否会与跟踪目标发生碰撞【防盗标记–盒子君hzj】。输入的信息包括车辆的跟踪轨迹，车辆的速度信息和目标跟踪信息

## **总结**

## **参考资料**

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
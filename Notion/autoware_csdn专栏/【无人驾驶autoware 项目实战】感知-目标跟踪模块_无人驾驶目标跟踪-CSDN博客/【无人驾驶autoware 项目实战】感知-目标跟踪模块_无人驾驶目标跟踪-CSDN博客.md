---
Created: 2023-12-18T14:14
Updated: 2023-12-18T14:14
URL: https://blog.csdn.net/qq_35635374/article/details/121859926
---
# **【无人驾驶autoware 项目实战】感知-目标跟踪模块**

![[images/original 5.png|original 5.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newCurrentTime2 4.png|newCurrentTime2 4.png]]

于 2021-12-10 16:44:26 发布

![[images/articleReadEyes2 5.png|articleReadEyes2 5.png]]

阅读量1.6k

收藏  
  
  
5  

![[images/tobarCollect2 5.png|tobarCollect2 5.png]]

![[images/newHeart2023Black 5.png|newHeart2023Black 5.png]]

点赞数  
3  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==目标跟踪==](https://so.csdn.net/so/search/s.do?q=%E7%9B%AE%E6%A0%87%E8%B7%9F%E8%B8%AA&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 4.|resize2Cm_fixed2Ch_2242Cw_224 4.]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 4.png|studyVipIcon 4.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121859926#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121859926#_13)
- [ ] [一、功能](https://blog.csdn.net/qq_35635374/article/details/121859926#_28)
- [ ] [二、输入](https://blog.csdn.net/qq_35635374/article/details/121859926#_35)
- [ ] [三、输出](https://blog.csdn.net/qq_35635374/article/details/121859926#_40)
- [ ] [四、算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121859926#_49)
- [ ]
    - [ ] [1.【image_tracker】](https://blog.csdn.net/qq_35635374/article/details/121859926#1image_tracker_50)
    - [ ] [2.【object_tracter】](https://blog.csdn.net/qq_35635374/article/details/121859926#2object_tracter_56)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121859926#_73)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121859926#_81)

---

## **前言**

  
  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**【无人驾驶autoware 项目实战】感知-目标跟踪模块**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2.]]

## **一、功能**

在实现检测的基础上，实现再图像中的目标跟踪，实现在真实场景基于卡尔曼滤波器、粒子滤波器的目标跟踪  
  
.  
  
.

## **二、输入**

【objects class】各种融合的跟踪目标检测类别  
  
.  
  
.

## **三、输出**

【objects ID】要跟踪的目标类别ID  
  
.  
  
.

## **四、算法流程实现**

### **1.【image_tracker】**

目标跟踪：输入各种融合的目标类别objects class，算法基于Beyond Pixels，图像上的目标跟踪结果被投影到3D空间，经过目标跟踪，输出要跟踪的目标类别ID  
  
立个flag…效果、算法原理、源码我在整理一次再更~  
  
.

### **2.【object_tracter】**

预测检测目标的下一步位置，跟踪的结果可以被进一步用于目标行为分析和目标速度分析【防盗标记–盒子君hzj】。跟踪算法主要是基于卡尔曼滤波器。  
  
我们解决这个跟踪问题使用了两种算法：  
  
卡尔曼滤波是在线性系统下使用的假设我们的自动驾驶汽车跟踪时匀速行驶它的计算量是重量轻，适合实时处理  
  
粒子过滤器可以适用于非线性跟踪场景我们的自动驾驶汽车和履带车辆正在移动。  
  
我们使用卡尔曼滤波器和粒子滤波器，这取决于给定场景。我们还将它们应用于二维（图像）平面和三维平面上的跟踪三维（点云）平面  
  
立个flag…效果、算法原理、源码我在整理一次再更~

## **总结**

## **参考资料**

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
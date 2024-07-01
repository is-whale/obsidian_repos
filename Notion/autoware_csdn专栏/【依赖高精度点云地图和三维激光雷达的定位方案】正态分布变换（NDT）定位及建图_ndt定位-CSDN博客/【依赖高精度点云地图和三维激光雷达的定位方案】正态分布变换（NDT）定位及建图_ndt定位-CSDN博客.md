---
Created: 2023-12-18T14:11
Updated: 2023-12-18T14:11
URL: https://blog.csdn.net/qq_35635374/article/details/121786885
---
# **【依赖高精度点云地图和三维激光雷达的定位方案】正态分布变换（NDT）定位及建图**

![[images/original 7.png|original 7.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newUpTime2 2.png|newUpTime2 2.png]]

已于 2022-05-03 14:58:33 修改

![[images/articleReadEyes2 7.png|articleReadEyes2 7.png]]

阅读量2.8k

收藏  
  
  
22  

![[images/tobarCollect2 7.png|tobarCollect2 7.png]]

![[images/newHeart2023Black 7.png|newHeart2023Black 7.png]]

点赞数  
4  

分类专栏：[==# 定位location==](https://blog.csdn.net/qq_35635374/category_11464501.html)[==# 地图mapping==](https://blog.csdn.net/qq_35635374/category_11464370.html)[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**同时被 3 个专栏收录**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 6.|resize2Cm_fixed2Ch_2242Cw_224 6.]]

![[newArrowDown1White.png]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 6.png|studyVipIcon 6.png]]

[**地图mapping**](https://blog.csdn.net/qq_35635374/category_11464370.html)

![[post-images/resize2Cm_fixed2Ch_2242Cw_224 1]]

19 篇文章  
24 订阅  
  
  
**¥9.90**~~¥99.00~~

==订阅专栏==超级会员免费看

![[images/studyVipIcon 6.png|studyVipIcon 6.png]]

[**定位location**](https://blog.csdn.net/qq_35635374/category_11464501.html)

![[post-images/resize2Cm_fixed2Ch_2242Cw_224 2]]

6 篇文章  
1 订阅  
  
  
**¥9.90**~~¥99.00~~

==订阅专栏==超级会员免费看

![[images/studyVipIcon 6.png|studyVipIcon 6.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121786885#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121786885#_13)
- [ ] [一、正态分布变换（NDT）定位解决的主要问题](https://blog.csdn.net/qq_35635374/article/details/121786885#NDT_26)
- [ ] [二、正态分布变换（NDT）定位的核心思想](https://blog.csdn.net/qq_35635374/article/details/121786885#NDT_33)
- [ ] [三、NDT算法流程](https://blog.csdn.net/qq_35635374/article/details/121786885#NDT_39)
- [ ]
    - [ ] [1.多元正态分布概念](https://blog.csdn.net/qq_35635374/article/details/121786885#1_40)
    - [ ] [2.网格化并计算正态分布参数](https://blog.csdn.net/qq_35635374/article/details/121786885#2_52)
    - [ ] [3.变换参数和最大似然](https://blog.csdn.net/qq_35635374/article/details/121786885#3_67)
- [ ] [四、使用C++实现NDT配准](https://blog.csdn.net/qq_35635374/article/details/121786885#CNDT_77)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121786885#_88)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121786885#_99)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**依赖高精度地图和激光雷达的定位技术–正态分布变换（NDT）定位**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容  
  
真正实现无人车的SLAM是非常困难的，作为交通工具，远距离的行驶会使得实时构建地图的偏差变大。所以，在已经拥有高精度地图的前提下去做无人车的定位，是更加现实，简单的。NDT就是一类利用已有的高精度地图和激光雷达实时测量数据实现高精度定位的技术。

## **一、**[**正态分布**](https://so.csdn.net/so/search?q=%E6%AD%A3%E6%80%81%E5%88%86%E5%B8%83&spm=1001.2101.3001.7020)**变换（NDT）定位解决的主要问题**

比较lidar扫描得到的[点云](https://so.csdn.net/so/search?q=%E7%82%B9%E4%BA%91&spm=1001.2101.3001.7020)和我们的地图点云，其中一个问题在于：lidar扫描得到的点云可能和地图的点云存在细微的区别，这里的偏差可能来自于测量误差，也有可能这个“场景”发生了一下变化（比如说行人，车辆）。NDT配准就是用于解决这些细微的偏差问题。

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_172Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

. .

## **二、正态分布变换（NDT）定位的核心思想**

NDT并没有比较地图点云和传感器点云中两个点云点与点之间的差距，而是先将参考点云（即高精度地图）转换为多维变量的正态分布，如果变换参数能使得两幅激光数据匹配的很好，那么变换点在参考系中的**概率密度**将会很大。因此，可以考虑用优化的方法求出使得概率密度之和最大的变换参数，此时两幅激光点云数据将匹配的最好  
  
.  
  
.

## **三、NDT算法流程**

### **1.多元正态分布概念**

我们知道，如果随机变量X满足正态分布（即 X∼N(μ,σ) ）,则其概率密度函数为： 其中的 μ 为正态分布的均值， σ2 为方差，这是对于维度 D=1 的情况而言的。对于多元正态分布而言，其概率密度函数可以表示为 其中 x 就表示均值向量，而 Σ 表示协方差矩阵,我们知道，协方差矩阵对角元素表示的是对应的元素的方差，非对角元素则表示对应的两个元素（行与列）的相关性。  
  
  
  
下图表示一个二元的正态分布 . .

![[post-images/12a2994795ff4c6fb0c2159552d5cccc.png]]

![[post-images/d9c7b2ff331245d1bf71feeb431f0c60.png]]

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_182Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

### **2.网格化并计算正态分布参数**

NDT算法的第一步就是将参考点云网格化（对于三维地图来说，即使用一个一个小立方体对整个空间的扫描点进行划分），对于每一个网格（cell）,基于网格内的点计算其概率密度函数（probability density function, PDF） 其中 表示一个网格内所有的扫描点  
  
  
  
那么一个网格的概率密度函数则为 使用正态分布来表示原本离散的点云有诸多好处，这种分块的（通过一个个cell）光滑的表示是连续可导的，每一个概率密度函数可以被认为是一个局部表面（local surface）的近似,它不但描述了这个表面在空间中的位置，同时还包含了这个表面的方向和光滑性等信息。下图是一个3D点云及其网格化效果: 上图中立方体的边长为 1 米，其中越明亮的位置表示概率越高。此外，局部表面的方向和光滑性则可以通过协方差矩阵的特征值和特征向量反映出来。我们以三维的概率密度函数为例，如果三个特征值很接近，那么这个正态分布描述的表面是一个球面，如果一个特征值远大于另外两个特征值，则这个正态分布描述的是一条线，如果一个特征值远小于另外两个特征值，则这个正态分布描述的是一个平面  
  
  
  
.  
  
.

![[post-images/091a4fdfe4b0446a9bf7d8368d1cade0.png]]

![[post-images/56cfe19bee094d51b694b6dfc914e051.png]]

![[post-images/6b6294351a544e10935eb91e5bcc09c0.png]]

![[images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 3.|watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 3.]]

### **3.变换参数和最大似然**

当使用NDT配准时，目标是找到当前扫描的姿态，使当前扫描的点位于参考扫描（3D地图）表面上的可能性最大化。那么我们需要优化的参数就是对当前扫描的点云的变换（旋转，平移等），我们使用一个变换参数 p→ 来描述。当前扫描为一个点云 X={x1,…,xn},给定扫描点集合 X 和变换参数 p→ ，令空间转换函数 T(p ,xk) 表示使用使用姿态变换 p→ 来移动点x→k ,结合之前的一组状态密度函数（每个网格都有一个PDF），那么最好的变换参数 p→ 应该是最大化似然函数的姿态变换： 那么最大化似然也就相当于最小化负对数似然 −logΘ; 到这里，就到了我们最熟悉的最优化的部分了。现在的任务就是使用优化算法来调整变换参数 p⃗ p→ 来最小化这个负对数似然。NDT算法使用牛顿法进行参数优化。我们不难看出，这里的概率密度函数 f(x⃗ )f(x→) 其实并不要求一定是正态分布，任何能够反映扫描表面的结构信息且对异常扫描点具有鲁棒性的概率密度函数都是可以的。 . .

![[post-images/94e0572366ad4f5fa18e893fdd27cf9f.png]]

![[post-images/583a45a90c0e4fdeb8f46d78474c50d1.png]]

## **四、使用C++实现NDT配准**

完整代码： [https://gitee.com/AdamShan/NDT_PCL_demo](https://gitee.com/AdamShan/NDT_PCL_demo)

.

.

## **总结**

退化场景中无法使用；  
  
不能在地图中去除动态障碍物产生的噪声；  
  
重定位需要给定初始值，【防盗标记–盒子君hzj】最好使用IMU，NDT对角度比较敏感；  
  
无回环检测，大环境建图会发生漂移。  
  
.  
  
.

## **参考资料**

[https://blog.csdn.net/AdamShan/article/details/79230612](https://blog.csdn.net/AdamShan/article/details/79230612)

（1）开源自动驾驶系统Autoware提供的Mapping模块

（2）论文与数学推导的理论

https://blog.csdn.net/AdamShan/article/details/79230612

http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.10.7059&rep=rep1&type=pdf

（3）Autoware的建图教程

https://blog.csdn.net/Travis_X/article/details/105455195

![[post-images/f7bd44daff76490e9f77d6d43cb224cb.png]]

![[post-images/4f97dc4547b145b897de2b6c9d409d02.png]]

![[post-images/a28ec44e071b44f59dd5edb62da929a8.png]]

![[post-images/2c84f0c11bed445ebcbe13a061821fce 2.png|2c84f0c11bed445ebcbe13a061821fce 2.png]]

![[post-images/8d425917cb594eb1aadfc1b00092c631.png]]

![[post-images/c0c2ff9d8dbe4b1db2026e02eccc91ef.png]]

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
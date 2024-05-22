---
Created: 2023-12-18T14:12
Updated: 2023-12-18T14:12
URL: https://blog.csdn.net/qq_35635374/article/details/121861608
---
# **【无人驾驶autoware 项目实战】autoware架构-manager**

![[images/original 13.png|original 13.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newCurrentTime2 10.png|newCurrentTime2 10.png]]

于 2021-12-10 17:38:17 发布

![[images/articleReadEyes2 13.png|articleReadEyes2 13.png]]

阅读量3.4k

收藏  
  
  
31  

![[images/tobarCollect2 13.png|tobarCollect2 13.png]]

![[images/newHeart2023Black 13.png|newHeart2023Black 13.png]]

点赞数  
6  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==架构==](https://so.csdn.net/so/search/s.do?q=%E6%9E%B6%E6%9E%84&t=all&o=vip&s=&l=&f=&viparticle=)[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 12.|resize2Cm_fixed2Ch_2242Cw_224 12.]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 12.png|studyVipIcon 12.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121861608#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121861608#_13)
- [ ] [（1）从Autoware的系统层进行剖析](https://blog.csdn.net/qq_35635374/article/details/121861608#1Autoware_25)
- [ ] [（2）从系统组件框图进行剖析](https://blog.csdn.net/qq_35635374/article/details/121861608#2_29)
- [ ] [（3）从算法的基本控制和数据流进行剖析](https://blog.csdn.net/qq_35635374/article/details/121861608#3_33)
- [ ] [（4）从Autoware的节点图进行剖析【较有用】](https://blog.csdn.net/qq_35635374/article/details/121861608#4Autoware_37)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121861608#_51)
- [ ]
    - [ ] [部署经验总结](https://blog.csdn.net/qq_35635374/article/details/121861608#_54)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121861608#_69)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**XXX**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

## **（1）从Autoware的系统层进行剖析**

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 8.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 8.]]

## **（2）从系统组件框图进行剖析**

这个框架少了高精度矢量地图的部分【SLAM建图部分–静态地图及矢量化，其他地图图层生成】

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 1 2.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 1 2.]]

## **（3）从算法的基本控制和数据流进行剖析**

![[watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_182Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

## **（4）从Autoware的节点图进行剖析【较有用】**

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2 2.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2 2.]]

## **总结**

Autoware仅仅是一个工具，实现的源码内容还是得靠自己的去学，学习源码原理参考百度，Autoware的整体是比较复杂的，整体解读清楚它的原理短时间是不可能实现的，所以别整体的源码去看，从自己需要的模块逐个突破

### **部署经验总结**

（1）autoware的bag数据包得改成自己得做验证，有实物传感器更好啦，没有的化自己搭gazebo仿真一样能拿到传感器数据 （2）autoware在ubuntu18.04也可以装1.12以上的版本【防盗标记–盒子君hzj】 （3）安装autoware从简单装起来，用到什么装什么 （4）装好autoware启动界面肯定亏出现花屏的现象，按照博客来pip装wx，再改文件就行 [https://www.cxybb.com/article/m0_46673077/115400045](https://www.cxybb.com/article/m0_46673077/115400045) [https://codeleading.com/article/10816031861/](https://codeleading.com/article/10816031861/)

（5）使用bag_demo运行起来的时候仅仅只有一个CPU在运行是正常的，后面用自己的bag数据包自己选择算法的时候CPU运行就可选了

.

.

.

## **参考资料**

（1）官方视频介绍（ROSCon 2017 ） [https://www.youtube.com/watch?v=XlXHLoIDohc](https://www.youtube.com/watch?v=XlXHLoIDohc)

（2）官网提供的 PPT 简介，文档说明

[https://github.com/CPFL/Autoware-Manuals](https://github.com/CPFL/Autoware-Manuals)

（3）autoware 的 gitlab 链接

[https://gitlab.com/autowarefoundation/autoware.ai/autoware](https://gitlab.com/autowarefoundation/autoware.ai/autoware)

（4）官方数据包rosbag下载（付费）

[https://data.tier4.jp](https://data.tier4.jp/)

像我一样没钱的可以考虑自己搭个gazebo的仿真环境就好，毕竟学生太…

（5）公司官网

[https://tier4.jp/](https://tier4.jp/)

（6）autoware 操作教程（很全）

[https://www.ncnynl.com/archives/201910/3401.html](https://www.ncnynl.com/archives/201910/3401.html)

（7）优酷上演示 autoware 各种 demo 的配置视频

[https://i.youku.com/i/UNDIxMDQ1MTkzNg==?spm=a2h0j.11185381.module_basic_dayu_sub.DLDDH2~A](https://i.youku.com/i/UNDIxMDQ1MTkzNg==?spm=a2h0j.11185381.module_basic_dayu_sub.DLDDH2~A)

（8）优酷 autoware 的中文介绍

[https://v.youku.com/v_show/id_XMzExNDQ0NzE2NA==.html?spm=a2hzp.8253869.0.0](https://v.youku.com/v_show/id_XMzExNDQ0NzE2NA==.html?spm=a2hzp.8253869.0.0)

（9）创客智造的教程

[https://www.ncnynl.com/archives/201910/3401.html](https://www.ncnynl.com/archives/201910/3401.html)

（10）PIX教程

[https://www.cnblogs.com/hgl0417/p/11844135.html](https://www.cnblogs.com/hgl0417/p/11844135.html)

[https://www.cnblogs.com/hgl0417/p/14617025.html](https://www.cnblogs.com/hgl0417/p/14617025.html)

（11）autoware的gazebo仿真博客教程【知道效果可以学习源码原理】

假设你已经安装好了Autoware，Autoware源码中其实已经配置有Gazebo仿真环境，当然你也可以根据自己的需要另外下载自动驾驶汽车的仿真模型。该汽车模型已经默认配置好了Velodyne HDL-32E 激光雷达、IMU和相机

在Gazebo仿真环境配置自动驾驶汽车：[https://blog.csdn.net/Travis_X/article/details/105418119](https://blog.csdn.net/Travis_X/article/details/105418119)

给仿真环境中的自动驾驶汽车更换或添加传感器：[https://blog.csdn.net/Travis_X/article/details/105418550](https://blog.csdn.net/Travis_X/article/details/105418550)

使用NDT构建点云地图：[https://blog.csdn.net/Travis_X/article/details/105455195](https://blog.csdn.net/Travis_X/article/details/105455195)

使用Hybrid a*进行路径规划：[https://blog.csdn.net/Travis_X/article/details/105949471](https://blog.csdn.net/Travis_X/article/details/105949471)

使用聚类算法作物体检测：[https://blog.csdn.net/Travis_X/article/details/106113427](https://blog.csdn.net/Travis_X/article/details/106113427)

使用Pure Pursuit和MPC进行路径追踪：[https://blog.csdn.net/Travis_X/article/details/106116998](https://blog.csdn.net/Travis_X/article/details/106116998)

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[Python入门技能树](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/python/?utm_source=csdn_ai_skill_tree_blog)_379603_ _人正在系统学习中_

 _显示推荐内容_
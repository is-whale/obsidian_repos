---
Created: 2023-12-18T14:12
Updated: 2023-12-18T14:12
URL: https://blog.csdn.net/qq_35635374/article/details/123558397
---
# **【autoware巡航、避障、视觉目标检测】步骤简单记录**

![[original.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[newCurrentTime2.png]]

于 2022-03-17 19:40:32 发布

![[articleReadEyes2.png]]

阅读量3.1k

收藏  
  
  
28  

![[tobarCollect2.png]]

![[newHeart2023Black.png]]

点赞数  
6  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==目标检测==](https://so.csdn.net/so/search/s.do?q=%E7%9B%AE%E6%A0%87%E6%A3%80%E6%B5%8B&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==计算机视觉==](https://so.csdn.net/so/search/s.do?q=%E8%AE%A1%E7%AE%97%E6%9C%BA%E8%A7%86%E8%A7%89&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[post-images/resize2Cm_fixed2Ch_2242Cw_224]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[studyVipIcon.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/123558397#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/123558397#_13)
- [ ] [一、线性地盘的改造ECU](https://blog.csdn.net/qq_35635374/article/details/123558397#ECU_24)
- [ ] [二、录制数据包](https://blog.csdn.net/qq_35635374/article/details/123558397#_32)
- [ ] [三、NDT建图--NDT_mapping](https://blog.csdn.net/qq_35635374/article/details/123558397#NDTNDT_mapping_39)
- [ ] [四、NDT定位--NDT_matching](https://blog.csdn.net/qq_35635374/article/details/123558397#NDTNDT_matching_47)
- [ ] [五、航线录制（waypoint_saver）](https://blog.csdn.net/qq_35635374/article/details/123558397#waypoint_saver_51)
- [ ] [六、航线加载（waypoint_loader）+pure_persuit跟踪巡航](https://blog.csdn.net/qq_35635374/article/details/123558397#waypoint_loaderpure_persuit_56)
- [ ] [七、A*动态路径规划（A-star）](https://blog.csdn.net/qq_35635374/article/details/123558397#AAstar_61)
- [ ] [八、yolo v3的目标检测](https://blog.csdn.net/qq_35635374/article/details/123558397#yolo_v3_68)
- [ ] [九、组合自己想要的功能](https://blog.csdn.net/qq_35635374/article/details/123558397#_72)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**【autoware巡航、避障、视觉目标检测】步骤简单记录**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

## **一、线性地盘的改造**[**ECU**](https://so.csdn.net/so/search?q=ECU&spm=1001.2101.3001.7020)

具体过程参考我的博客

[https://blog.csdn.net/qq_35635374/article/details/121180713](https://blog.csdn.net/qq_35635374/article/details/121180713)

.

.

## **二、录制数据包**

遥控车辆，或者手动驾驶录制数据包bag（在manager界面上操作）车模型的加载，设置各个传感器的TF坐标，主要录制传感器数据，视觉图像、激光点云，IMU数据等等

录制的数据类型参考我这篇博客 [https://blog.csdn.net/qq_35635374/article/details/121859664](https://blog.csdn.net/qq_35635374/article/details/121859664) . .

## **三、**[**NDT**](https://so.csdn.net/so/search?q=NDT&spm=1001.2101.3001.7020)**建图–NDT_mapping**

使用上述数据包，下采样降低数据量，点云一些地面处理，采用DNT算法建立点云地图，并把PCB地图保存下来

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

.

.

## **四、NDT定位–NDT_matching**

实时激光雷达的数据数据与NDT建立的地图进行匹配，注意的是车辆当前停止的位置要和录制建立地图的其实位置重合，要不然匹配不上

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 1]]

## **五、航线录制（waypoint_saver）**

加载建立好的地图，运行NDT定位，录制航线到csv文件中（路径包含速度、位置、姿态） . .

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2]]

## **六、航线加载（waypoint_loader）+pure_persuit跟踪巡航**

航线加载（waypoint_loader）后rviz出现轨迹，选择lane_rule、lane_select的任务规划，再选择a*是否避障，再选择twist指令滤波，最后使用pure_persuit跟踪，查看can总线的数据是否正常（candump any） . .

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 3]]

## **七、A*动态**[**路径规划**](https://so.csdn.net/so/search?q=%E8%B7%AF%E5%BE%84%E8%A7%84%E5%88%92&spm=1001.2101.3001.7020)**（A-star）**

基于栅格地图做避障需要生成栅格地图（costmap_generator），设置costmap的相关参数

后面的步骤和巡航的步骤是一致的

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 4]]

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 5]]

## **八、yolo v3的目标检测**

勾选上vision_darknet_yolo3，在rviz上选择对应图像的topic，即可出现检测的效果，可以使用候选框的大小表示距离车辆的距离

![[post-images/watermark2Ctype_d3F5LXplbmhlaQ2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 6]]

## **九、组合自己想要的功能**

把上述NDT建图与定位、航线录制、巡航、避障、视觉检测加到一起启动就行了、  
  
.  
  
.

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23317_ _人正在系统学习中_

 _显示推荐内容_
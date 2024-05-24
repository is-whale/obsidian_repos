---
Created: 2023-12-18T14:14
Updated: 2023-12-18T14:14
URL: https://blog.csdn.net/qq_35635374/article/details/121860093
---
# **【无人驾驶autoware 项目实战】规划-运动规划motion planning**

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

[**无人驾驶相关Autoware**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121860093#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121860093#_13)
- [ ] [一、功能](https://blog.csdn.net/qq_35635374/article/details/121860093#_28)
- [ ] [二、输入](https://blog.csdn.net/qq_35635374/article/details/121860093#_36)
- [ ]
    - [ ] [【temporal waypoint】](https://blog.csdn.net/qq_35635374/article/details/121860093#temporal_waypoint_37)
    - [ ] [【current velocity】](https://blog.csdn.net/qq_35635374/article/details/121860093#current_velocity_41)
    - [ ] [【current pose】](https://blog.csdn.net/qq_35635374/article/details/121860093#current_pose_45)
- [ ] [三、输出](https://blog.csdn.net/qq_35635374/article/details/121860093#_50)
- [ ]
    - [ ] [【final waypoint】](https://blog.csdn.net/qq_35635374/article/details/121860093#final_waypoint_51)
- [ ] [四、算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121860093#_56)
- [ ]
    - [ ] [【velovity_planner(velocity setting)】](https://blog.csdn.net/qq_35635374/article/details/121860093#velovity_plannervelocity_setting_57)
    - [ ] [【astar_planner】](https://blog.csdn.net/qq_35635374/article/details/121860093#astar_planner_66)
    - [ ] [【adas_lattice_planner】](https://blog.csdn.net/qq_35635374/article/details/121860093#adas_lattice_planner_74)
    - [ ] [【open_planner】](https://blog.csdn.net/qq_35635374/article/details/121860093#open_planner_83)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121860093#_102)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121860093#_110)

---
## **前言**

  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**规划-运动规划motion planning**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 14.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 14.]]

## **一、功能**

planning负责根据给定的一段temporal waypoint轨迹生成局部可行轨迹全局轨迹，考虑到车辆状态，三维地图所示的位置及其局部的障碍物分布情况，周围对象、交通规则和期望的目标，在局部环境下，规划出来一条最优的路径  
  
.  
  
.

## **二、输入**

### **【temporal waypoint】**

一段带有速度的轨迹  
  
.  
  
.

### **【current velocity】**

定位与车底盘CAN里程计反馈融合得到的车辆当前速度  
  
.  
  
.

### **【current pose】**

定位模块提供的车辆当前位姿  
  
.

## **三、输出**

### **【final waypoint】**

无人车执行的最终轨迹  
  
.  
  
.

## **四、算法流程实现**

### **【velovity_planner(velocity setting)】**

在base waypoint的基础上，截取的一小段路径，并进行速度规划/设置，得到一段带有速度的轨迹temporal waypoint，更新车辆速度信息，注意到给定跟踪的waypoint里面是带有速度信息的，这个模块就是根据车辆的实际状态进一步修正速度信息，以便于实现在停止线前面停止下来或者加减速等等

算法原理我写在了另外的博客，转战一下： [【速度轨迹优化】double S曲线进行机械臂轨迹和无人驾驶路径的速度都规划](https://blog.csdn.net/qq_35635374/article/details/121380217) [【Trajectory optimization】对轨迹点进行设置、插值、平滑方法](https://blog.csdn.net/qq_35635374/article/details/120936564) . .

### **【astar_planner】**

实现Hybrid-State A*查找算法，生成从现在位置到指定位置的可行轨迹，【防盗标记–盒子君hzj】这个模块可以实现**避障**，或者在给定waypoint下的急转弯，也包括在**自由空间内的自动停车**

算法原理我写在了另外的博客，转战一下： [【路径生成–图搜索的方法】贪心算法、Dijkstra和A](https://blog.csdn.net/qq_35635374/article/details/120625182)[_类路径搜索算法【经典】_](https://blog.csdn.net/qq_35635374/article/details/120625182) [【路径生成–更复杂启发式的图搜索算法】hybird A算法（较常用）](https://blog.csdn.net/qq_35635374/article/details/120686552) . .

### **【adas_lattice_planner】**

实现了State Lattice规划算法，事先定义好的参数列表和语义地图信息，基于mini_jerk样条曲线实现局部路径生成与优化，在当前位置前方产生了多条可行路径，最终得到无人车执行的最终轨迹final waypoint，可以被用来进行障碍物**避障**或**车道线换道**

算法原理我写在了另外的博客，转战一下： [【路径生成+轨迹优化–无人驾驶规划器】Lattice planner算法](https://blog.csdn.net/qq_35635374/article/details/120684925) [【五次样条路径生成并Frenet坐标系解耦轨迹优化】基于Frenet坐标系的无人车轨迹生成并优化方法](https://blog.csdn.net/qq_35635374/article/details/121773984)

.

.

### **【open_planner】**

实现路径生成与优化

算法原理我写在了另外的博客，转战一下： [【路径生成 + 轨迹优化–无人驾驶场景规划器】OpenPlanner算法](https://blog.csdn.net/qq_35635374/article/details/120712791)

.

.

---

## **总结**

## **参考资料**

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
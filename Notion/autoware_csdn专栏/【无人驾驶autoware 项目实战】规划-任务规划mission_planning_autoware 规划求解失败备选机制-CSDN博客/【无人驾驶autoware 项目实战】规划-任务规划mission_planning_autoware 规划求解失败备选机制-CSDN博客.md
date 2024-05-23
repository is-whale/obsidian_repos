---
Created: 2023-12-18T14:14
Updated: 2023-12-18T14:14
URL: https://blog.csdn.net/qq_35635374/article/details/121860065
---
# **【无人驾驶autoware 项目实战】规划-任务规划mission_planning**


分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121860065#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121860065#_13)
- [ ] [一、功能](https://blog.csdn.net/qq_35635374/article/details/121860065#_28)
- [ ] [二、输入](https://blog.csdn.net/qq_35635374/article/details/121860065#_37)
- [ ]
    - [ ] [【ADAS 矢量地图】](https://blog.csdn.net/qq_35635374/article/details/121860065#ADAS__38)
- [ ] [三、输出](https://blog.csdn.net/qq_35635374/article/details/121860065#_44)
- [ ]
    - [ ] [【base waypoints】](https://blog.csdn.net/qq_35635374/article/details/121860065#base_waypoints_45)
- [ ] [四、算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121860065#_51)
- [ ]
    - [ ] [【route planner】](https://blog.csdn.net/qq_35635374/article/details/121860065#route_planner_52)
    - [ ] [【lane planner】](https://blog.csdn.net/qq_35635374/article/details/121860065#lane_planner_56)
    - [ ] [【waypoint_planner】](https://blog.csdn.net/qq_35635374/article/details/121860065#waypoint_planner_60)
    - [ ] [【waypoint_maker】](https://blog.csdn.net/qq_35635374/article/details/121860065#waypoint_maker_64)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121860065#_76)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121860065#_84)

---

## **前言**

  
  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**【无人驾驶autoware 项目实战】规划-任务规划mission_planning**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容
## **一、功能**

在全局中，指定一个起点，一个终点，通过十字路口全局规划得到一条全局静态路径，一旦分配了全局路径，本地马上启动运动规划器  
  
autoware的**规划任务计划是半自主的**，在更复杂的场景中，例如停车和恢复工作错误，司机需要监督路径  
  
## **二、输入**

### **【ADAS 矢量地图】**

根据高精度点云地图，图像地图，人工绘制的语义地图（一般由公司专门去做的，相当于给定了.csv文件的航线lane、交通灯等等更多的静态地图信息）  

## **三、输出**

### **【base waypoints】**

全局航线，全局航线本来就是包含在了ADAS 矢量地图的csv文件中的  

## **四、算法流程实现**

### **【route planner】**

路线规划，寻找到达目标地点的全局路径并发布的一系列十字路口结果，路径由道路route网中的一系列十字路口组成，大体决定走那一条路线route，每条路线拥有多条车道线lane  

### **【lane planner】**

车道规划，根据route_planner发布的一系列十字路口结果，确定全局路径由哪些lane组成，并发布到达目的地的一系列waypoint数组，lane是由一系列waypoint点组成。  

### **【waypoint_planner】**

用于产生到达目的地的一系列waypont点，【防盗标记–盒子君hzj】它与lane_planner的不同之处在于它是发布单一的到达目的地的waypoint路径,而lane_planner是发布到达目的地的一系列waypoint数组  

### **【waypoint_maker】**

相当于是航线录制的功能包 。waypoint_maker 是一个**保存和加载手动制作的waypoint文件的工具**。为了保存waypoint到文件里，需要手动驾驶车辆并开启定位模块，然后记录车辆的一系列定位信息以及速度信息， 被记录的信息汇总成为一个路径文件，之后可以加载这个本地文件，并发布需要跟踪的轨迹路径信息给其他规划模块

## **总结**

## **参考资料**

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_
---
Created: 2023-12-18T14:06
Updated: 2023-12-18T14:27
URL: https://blog.csdn.net/qq_35635374/article/details/124654200
---
## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/124654200#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/124654200#_13)
- [ ] [决策规划模块功能介绍](https://blog.csdn.net/qq_35635374/article/details/124654200#_26)
- [ ]
    - [ ] [1.规划常用任务](https://blog.csdn.net/qq_35635374/article/details/124654200#1_29)
    - [ ]
        - [ ] [1.地图中点到点的全局路径规划（道路级别）](https://blog.csdn.net/qq_35635374/article/details/124654200#1_31)
        - [ ] [2.在行进过程中的局部路径规划（轨迹级别）](https://blog.csdn.net/qq_35635374/article/details/124654200#2_36)
- [ ] [一、全局规划（mission_planner）](https://blog.csdn.net/qq_35635374/article/details/124654200#mission_planner_51)
- [ ]
    - [ ] [方案一、lane_planner（使用vector_map）](https://blog.csdn.net/qq_35635374/article/details/124654200#lane_plannervector_map_52)
    - [ ]
        - [ ] [（1）规划原理介绍](https://blog.csdn.net/qq_35635374/article/details/124654200#1_55)
        - [ ] [（2）规划代码解析及注释【重点】](https://blog.csdn.net/qq_35635374/article/details/124654200#2_89)
        - [ ]
            - [ ] [（1）lane_navi](https://blog.csdn.net/qq_35635374/article/details/124654200#1lane_navi_95)
            - [ ] [（2）lane_rule](https://blog.csdn.net/qq_35635374/article/details/124654200#2lane_rule_101)
            - [ ] [（3）lane_select](https://blog.csdn.net/qq_35635374/article/details/124654200#3lane_select_112)
            - [ ] [（4）lane_stop](https://blog.csdn.net/qq_35635374/article/details/124654200#4lane_stop_181)
    - [ ] [方案二、freespace_planner(使用costmap，astar做全局点到点规划)](https://blog.csdn.net/qq_35635374/article/details/124654200#freespace_plannercostmapastar_196)
    - [ ]
        - [ ] [（1）规划原理流程介绍](https://blog.csdn.net/qq_35635374/article/details/124654200#1_200)
        - [ ] [（2）规划代码解析及注释](https://blog.csdn.net/qq_35635374/article/details/124654200#2_216)
        - [ ]
            - [ ] [（1）astar算法的前置决策astar_navi](https://blog.csdn.net/qq_35635374/article/details/124654200#1astarastar_navi_223)
            - [ ] [（2）astar算法的实现astar_search](https://blog.csdn.net/qq_35635374/article/details/124654200#2astarastar_search_245)
            - [ ] [（3）局部代价地图生成 costmap_generator](https://blog.csdn.net/qq_35635374/article/details/124654200#3_costmap_generator_257)
    - [ ] [方案三、op_global_planner(使用vector_map)](https://blog.csdn.net/qq_35635374/article/details/124654200#op_global_plannervector_map_288)
- [ ] [二、局部规划（motion_planner）](https://blog.csdn.net/qq_35635374/article/details/124654200#motion_planner_317)
- [ ]
    - [ ] [方案一、waypoints_maker](https://blog.csdn.net/qq_35635374/article/details/124654200#waypoints_maker_319)
    - [ ]
        - [ ] [（1）waypoint_loader节点（加载vector_map的csv路点文件）](https://blog.csdn.net/qq_35635374/article/details/124654200#1waypoint_loadervector_mapcsv_336)
        - [ ] [（2）waypoint_replanner节点（一般在waypoint_loader后进行速度规划）](https://blog.csdn.net/qq_35635374/article/details/124654200#2waypoint_replannerwaypoint_loader_347)
        - [ ] [（3）waypoint_saver节点（录制航线生成csv文件）](https://blog.csdn.net/qq_35635374/article/details/124654200#3waypoint_savercsv_384)
        - [ ] [（4）waypoint_extractor节点(加载录制的航线)](https://blog.csdn.net/qq_35635374/article/details/124654200#4waypoint_extractor_402)
        - [ ] [（5）waypoint_creator节点(根据原来加载的路径点进行路径插值、路径平滑)](https://blog.csdn.net/qq_35635374/article/details/124654200#5waypoint_creator_412)
        - [ ] [（6）waypoint_marker_publisher节点](https://blog.csdn.net/qq_35635374/article/details/124654200#6waypoint_marker_publisher_420)
        - [ ] [（7）waypoint_velocity_visualizer节点](https://blog.csdn.net/qq_35635374/article/details/124654200#7waypoint_velocity_visualizer_427)
    - [ ] [方案二、waypoint_planner（astar做局部规划）](https://blog.csdn.net/qq_35635374/article/details/124654200#waypoint_plannerastar_438)
    - [ ]
        - [ ] [（1）规划原理介绍](https://blog.csdn.net/qq_35635374/article/details/124654200#1_441)
        - [ ] [（2）规划代码解析及注释](https://blog.csdn.net/qq_35635374/article/details/124654200#2_460)
        - [ ]
            - [ ] [（1）astar_avoid节点](https://blog.csdn.net/qq_35635374/article/details/124654200#1astar_avoid_467)
            - [ ] [（2）astar算法的实现astar_search节点](https://blog.csdn.net/qq_35635374/article/details/124654200#2astarastar_search_480)
            - [ ] [（3）Velocity_set速度规划模块](https://blog.csdn.net/qq_35635374/article/details/124654200#3Velocity_set_486)
    - [ ] [方案三、open_planner](https://blog.csdn.net/qq_35635374/article/details/124654200#open_planner_497)
    - [ ]
        - [ ] [（1）规划原理流程介绍](https://blog.csdn.net/qq_35635374/article/details/124654200#1_502)
        - [ ] [（2）规划代码解析及注释](https://blog.csdn.net/qq_35635374/article/details/124654200#2_527)
        - [ ]
            - [ ] [（1）op_common_params（主要）](https://blog.csdn.net/qq_35635374/article/details/124654200#1op_common_params_531)
            - [ ] [（2）op_trajectory_evaluator（主要）](https://blog.csdn.net/qq_35635374/article/details/124654200#2op_trajectory_evaluator_533)
            - [ ] [（3）op_trajectory_generator（主要）](https://blog.csdn.net/qq_35635374/article/details/124654200#3op_trajectory_generator_535)
            - [ ] [（4）op_behavior_selector（可选）](https://blog.csdn.net/qq_35635374/article/details/124654200#4op_behavior_selector_537)
            - [ ] [（5）op_motion_predictor（可选）](https://blog.csdn.net/qq_35635374/article/details/124654200#5op_motion_predictor_539)
    - [ ] [方案四、lattice_planner](https://blog.csdn.net/qq_35635374/article/details/124654200#lattice_planner_552)
    - [ ]
        - [ ] [（1）规划原理流程介绍](https://blog.csdn.net/qq_35635374/article/details/124654200#1_553)
        - [ ] [（2）规划代码解析及注释](https://blog.csdn.net/qq_35635374/article/details/124654200#2_558)
        - [ ]
            - [ ] [（1）lattice_trajectory_gen（主要）](https://blog.csdn.net/qq_35635374/article/details/124654200#1lattice_trajectory_gen_559)
            - [ ] [（2）lattice_twist_convert（主要）](https://blog.csdn.net/qq_35635374/article/details/124654200#2lattice_twist_convert_560)
            - [ ] [（3）path_select（可选）](https://blog.csdn.net/qq_35635374/article/details/124654200#3path_select_561)
            - [ ] [（4）lattice_velocity_set（可选）](https://blog.csdn.net/qq_35635374/article/details/124654200#4lattice_velocity_set_562)
- [ ] [三、路径跟踪control](https://blog.csdn.net/qq_35635374/article/details/124654200#control_574)
- [ ]
    - [ ] [【运动控制算法概述】](https://blog.csdn.net/qq_35635374/article/details/124654200#_575)
    - [ ] [方案一、pure_persuit纯跟踪算法](https://blog.csdn.net/qq_35635374/article/details/124654200#pure_persuit_584)
    - [ ] [方案二、模型预测控制mpc算法](https://blog.csdn.net/qq_35635374/article/details/124654200#mpc_593)
    - [ ] [twist_filter控制指令滤波输出](https://blog.csdn.net/qq_35635374/article/details/124654200#twist_filter_633)
- [ ] [四、行为决策decision](https://blog.csdn.net/qq_35635374/article/details/124654200#decision_639)
- [ ]
    - [ ] [方案一：state_machine有限状态机](https://blog.csdn.net/qq_35635374/article/details/124654200#state_machine_641)
    - [ ] [方案二：decision_make](https://blog.csdn.net/qq_35635374/article/details/124654200#decision_make_648)
    - [ ]
        - [ ] [车辆状态VehicleStates](https://blog.csdn.net/qq_35635374/article/details/124654200#VehicleStates_655)
        - [ ] [全局航线状态MissionStates](https://blog.csdn.net/qq_35635374/article/details/124654200#MissionStates_657)
        - [ ] [局部航线状态MotionStates](https://blog.csdn.net/qq_35635374/article/details/124654200#MotionStates_659)
        - [ ] [行为和运动状态BehaviorStates](https://blog.csdn.net/qq_35635374/article/details/124654200#BehaviorStates_665)
    - [ ] [方案三：planner_selector](https://blog.csdn.net/qq_35635374/article/details/124654200#planner_selector_676)
- [ ] [机器人/自动驾驶的决策、规划、控制算法工程师的工作要求](https://blog.csdn.net/qq_35635374/article/details/124654200#_682)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/124654200#_702)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/124654200#_710)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！

本文先对**【[autoware](https://so.csdn.net/so/search?q=autoware&spm=1001.2101.3001.7020)的决策规划模块】**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章

---

提示：以下是本篇文章正文内容

## **决策规划模块功能介绍**

![[post-images/3c5af986faa4474b8c9599e7459d3a6e.png]]

### **1.规划常用任务**

### **1.地图中点到点的全局路径规划（道路级别）**

（场景很多，要一个一个场景的突破去做）  
  
.  
  
.

### **2.在行进过程中的局部路径规划（轨迹级别）**

（1）根据[红绿灯](https://so.csdn.net/so/search?q=%E7%BA%A2%E7%BB%BF%E7%81%AF&spm=1001.2101.3001.7020)信号的车辆启停

（2）前方障碍物的减速、[避障](https://so.csdn.net/so/search?q=%E9%81%BF%E9%9A%9C&spm=1001.2101.3001.7020)、停止

（3）换道

（4）跟车

（5）危险情况的紧急制动

转弯、倒车、换道、巡航、避障都是决策给出信息的，同时感知、地图、定位也有辅助

.

.

---

## **一、全局规划（mission_planner）**

### **方案一、lane_planner（使用vector_map）**

**巡航换道停障的规划方案【车道级规划–全局静态轨迹规划】**  
  
【lane_planner、waypoint_maker两个功能包的配合–实现从车道的角度进行规划】

### **（1）规划原理介绍**

【lane_planner、waypoint_maker两个功能包的配合–实现从车道的角度进行规划】  
  
  
  
lane_planner功能包对应的节点  
  
  
`lane_navi lane_rule lane_select lane_stop` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4`  
  
waypoint_maker功能包对应的节点  
  
  
`waypoints_loader waypoints_make_publish` `•` `[ ] 1` `•` `[ ] 2`  
  
**lane_planner的功能是输出一条整体的全局路径/base_waypoints**  
  
（1）vector_map给定的全局路径的获取方式可以通过lane_planner中的**lane_navi**解析得到，  
  
也可以通过waypoints_make中的**waypoints_loader**把path.txt解析出来，  
  
还可以通过Astar等全局规划出来的waypoints得到(其他方案)  
  
（2）得到/lane_waypoints_array就是我们之前给定的全局轨迹，可以仅有一条也可以有多条（主要看vector_map和path.txt提供了几条可供选择的候选路路径）  
  
（3）waypoints_make中的**waypoints_make_publish**把/lane_waypoints_array话题可视化到rviz中  
  
（4）lane_planner中的**lane_rule**根据红绿灯情况输出各种速度不同的路径轨迹，其中/red_waypoints_array的速度为0，/green_waypoints_array为正常的速度，/traffic_waypoints_array为交规指定的最终执行速度  
  
（5）lane_planner中的**lane_select**根据vector_map地图中的左右转信息决定车辆换道，走那一条路线  
  
经过上述一系列操作，最终输出一条可执行的base_waypoints,base_waypoints就是最终执行的全局路径，局部路径都是基于这条base_waypoints进行的  
  
.

![[post-images/d9579aa847af49859bfb96d7e72eb908.png]]

### **（2）规划代码解析及注释【重点】**

【rqt_graph可以查看核对一下的】  
  
我把注释后的代码上传到给github了，从new_mission_planning.launch看起

### **（1）lane_navi**

功能：  
  
  
`解析vector_map的路点信息` `•` `[ ] 1`

### **（2）lane_rule**

功能：  
  
  
`根据红绿灯情况输出各种速度不同的路径轨迹， 其中 /red_waypoints_array的速度为0， /green_waypoints_array为正常的速度， /traffic_waypoints_array为交规指定的最终执行速度` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5`

### **（3）lane_select**

功能：  
  
  
`1.接受多条路径 1.计算所有路径的当前位置的最近傍点 1.最近傍点的车道和现在行驶的路线设定 1.检测当前路径的左右路径 1.将当前路径最近傍点的车道变更标志作为该路径的车道变更标志保持 1.寻找最近邻右转或左转的标志点，生成艾尔米内插的路线，将该点和预定变更车道的车道的目标点定义为车道变更用的路径 1.不变更车道时，将当前路径、最近傍点、车道变更标志分别publish 1.更改车道时，分别对车道变更用的路径、与之相对的最近傍点、车道变更标志进行publish` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5` `•` `[ ] 6` `•` `[ ] 7` `•` `[ ] 8` `•` `[ ] 9` `•` `[ ] 10` `•` `[ ] 11` `•` `[ ] 12` `•` `[ ] 13` `•` `[ ] 14` `•` `[ ] 15`  
  
发布/订阅的信息  
  
  
`1. Subscribed Topics - traffic_waypoints_array (waypoint_follower/LaneArray) - current_pose (geometry_msgs/PoseStamped) - current_velocity (geometry_msgs/TwistStamped) - state (std_msgs/String) - config/lane_select (runtime_manager/ConfigLaneSelect) 1. Published Topics - base_waypoints (waypoint_follower/lane) - closest_waypoint (std_msgs/Int32) - change_flag (std_msgs/Int32) - lane_select_marker (visualization_msgs/MarkerArray) : for debug` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5` `•` `[ ] 6` `•` `[ ] 7` `•` `[ ] 8` `•` `[ ] 9` `•` `[ ] 10` `•` `[ ] 11` `•` `[ ] 12` `•` `[ ] 13` `•` `[ ] 14`  
  
修改的参数：  
  
  
`- Distance threshold to neighbor lanes<br> 表示检测当前路径周围有效车道时的阈值。距离这个阈值远的车道不识别为车道。 - Lane Change Interval After Lane Merge 表示进行了车道变更后跑了几米再能变更车道的值。 - Lane Change Target Ratio 将预定变更车道的车道上的目标点定义成与速度（m/s）成比例的距离时使用的值。 目标点探索的起点是在预定变更车道的车道上，有右转或左转的车道变更标志的点的最近傍点。 - Lane Change Target Minimum 表示到预定变更车道上的目标点为止的最低距离。 目标点探索的起点是在预定变更车道的车道上，有右转或左转的车道变更标志的点的最近傍点。 - Vector Length of Hermite Curve 表示用电子表格曲线补充时矢量的大小。` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5` `•` `[ ] 6` `•` `[ ] 7` `•` `[ ] 8` `•` `[ ] 9` `•` `[ ] 10` `•` `[ ] 11` `•` `[ ] 12` `•` `[ ] 13` `•` `[ ] 14` `•` `[ ] 15` `•` `[ ] 16` `•` `[ ] 17` `•` `[ ] 18` `•` `[ ] 19` `•` `[ ] 20` `•` `[ ] 21` `•` `[ ] 22` `•` `[ ] 23` `•` `[ ] 24` `•` `[ ] 25` `•` `[ ] 26` `•` `[ ] 27`

### **（4）lane_stop**

功能：  
  
  
`实现在vector_map的红绿灯下停车` `•` `[ ] 1`  
  
.  
  
.

### **方案二、freespace_planner(使用costmap，astar做全局点到点规划)**

Freespace planner软件包提供全局规划器节点，用于规划存在静态/动态障碍物的空间中的航路点。  
  
**手动指定目标点goal【全局动态轨迹级规划【动态生成代价地图实现】**

### **（1）规划原理流程介绍**

（1）【生成costmap的步骤】根据vector_map的/point_lane和雷达感知的障碍物【感知部分】，生成一张代价地图costmap【属于动态地图生成的范畴，在costmap_generator的功能实现】（当然如果有静态的costmap,这里也可以不生成costmap）  
  
  
  
（2）【在cost上进行astar图搜索的步骤】costmap_gennerator输出一张分割的占据栅格地图，同时**freespace_planner**功能包中的**astar_navi**是针对astar的一些前置决策，**astar_search**是astar算法的具体实现  
  
最终输出多条可执行的路径/lane_waypoints_array  
  
用法：  
  
**得到/lane_waypoints_array后可以继续运行lane_planner（这样做可以筛选出一条轨迹，在车辆还没有到达vector_map对应位置在结构化道路自行走一段）**  
  
**当然这个规划去本身是针对未建立vecor_map的自由场景的**  
  
  
  
.

![[post-images/20dce5d476c4486ea21d17b31f69542d.png]]

![[post-images/3b60339378c84d4d9b9b8cfe7d9ec3d0.png]]

### **（2）规划代码解析及注释**

freespace_planner功能包的节点  
  
  
`astar_navi astar_search` `•` `[ ] 1` `•` `[ ] 2`

### **（1）astar算法的前置决策astar_navi**

`astar_navi`是一款基于astar_search包中混合A*搜索算法的全局路径规划器。**该节点以恒定的周期执行规划**，并发布`lane_waypoints_array`  
  
订阅信息Subscriptions:  
  
  
`* /base_waypoints [autoware_msgs/Lane] * /closest_waypoint [std_msgs/Int32] * /current_pose [geometry_msgs/PoseStamped] * /current_velocity [geometry_msgs/TwistStamped] * /semantics/costmap_generator/occupancy_grid [nav_msgs/OccupancyGrid] * /obstacle_waypoint [std_msgs/Int32] * /tf [tf2_msgs/TFMessage] * /tf_static [tf2_msgs/TFMessage]` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5` `•` `[ ] 6` `•` `[ ] 7` `•` `[ ] 8`  
  
注意：  
  
astar_navi比较简单，原来的版本是一直进行局部规划的，改过的版本是仅仅进行局部规划一次的！  
  
.  
  
.

### **（2）astar算法的实现astar_search**

注意一下，autoware的astar算法版本是根据实际情况优化过的，毕竟图搜索的规划算法都被改烂了～  
  
  
  
astar并不是用pose进行搜寻的，而是用pose转index进行搜索的  
  
astar搜索autoware进行的改进，在expand中规定了搜索的扩展方向！  
  
.  
  
.

![[post-images/fc99e0431f484346a149fea391556d13.png]]

### **（3）局部代价地图生成 costmap_generator**

**功能：**  
  
  
`costmap_generator功能包读取“PointCloud”和/或“DetectedObjectArray”两个话题， 并创建“OccupencyGrid”和“GridMap”两种地图是可选的。` `•` `[ ] 1` `•` `[ ] 2`  
  
**订阅的话题：**  
  
`/points_no_ground` (sensor_msgs::PointCloud2) : from ray_ground_filter or compare map filter. It contains filtered points with the ground removed.  
  
`/prediction/moving_predictor/objects` (autoware_msgs::DetectedObjectArray): predicted objects from naive_motion_predict.  
  
`/vector_map`: from the VectorMap publisher. `/tf` to obtain the transform between the vector map(map_frame) and the sensor(sensor_frame) .  
  
.  
  
**发布的话题：**  
  
`/semantics/costmap` (grid_map::GridMap) is the output costmap, with values ranging from 0.0-1.0.  
  
`/semantics/costmap_generator/occupancy_grid` (nav_msgs::OccupancyGrid) is the output OccupancyGrid, with values ranging from 0-100.  
  
从new_manual_astar_planner.launch看起，相当于于用astar算法规划的路径地体路网文件txt/csv  
  
.  
  
.

### **方案三、op_global_planner(使用vector_map)**

**在地图上生成从起点到目标点的全局路径**。规划成本仅限于距离。该算法可以扩展到处理其他成本，例如在动态地图中右转和左转繁忙车道。它支持autoware矢量地图vector_map，并专门设计。  
  
Input：  
  
  
`加载vector map，从Rviz设置开始/目标，并保存到csv文件，如果禁用rviz参数，将从位于csv文件加载开始/目标。 * /initialpose [geometry_msgs::PoseWithCovarianceStamped] * /move_base_simple/goal [geometry_msgs::PoseStamped] * /current_pose [geometry_msgs::PoseStamped] * /current_velocity [geometry_msgs::TwistStamped] * /vector_map_info/*` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5` `•` `[ ] 6`  
  
Outputs：  
  
  
`* /lane_waypoints_array [autoware_msgs::LaneArray] * /global_waypoints_rviz [visualization_msgs::MarkerArray] * /op_destinations_rviz [visualization_msgs::MarkerArray] * /vector_map_center_lines_rviz [visualization_msgs::MarkerArray]` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4`  
  
**从起点到目标的全局路径，如果设置了多个目标，当车辆到达终点时，会自动重新规划一个目标。**  
  
.  
  
.

## **二、局部规划（motion_planner）**

### **方案一、waypoints_maker**

功能：录制航线、加载路径点、进行简单的速度规划、插值平滑、路径点可视化的简单操作  
  
waypoint_maker功能包包含以下几个节点  
  
  
`- waypoint_loader - waypoint_replanner - - waypoint_saver - waypoint_extractor - waypoint_creator - waypoint_marker_publisher - waypoint_velocity_visualizer` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5` `•` `[ ] 6` `•` `[ ] 7` `•` `[ ] 8` `•` `[ ] 9` `•` `[ ] 10`

### **（1）waypoint_loader节点（加载vector_map的csv路点文件）**

功能：把.csv文件的waypoints解析转换成ROS message type  
  
.csv文件的格式有三种  
  
  
`ver1： consist of x, y, z, velocity（no velocity in the first line） ver2： consist of x, y, z, yaw, velocity（no velocity in the first line） ver3：consist of x,y,z,yaw,velocity,change_flag（ category names are on the first line）` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3`  
  
.  
  
.

### **（2）waypoint_replanner节点（一般在waypoint_loader后进行速度规划）**

功能：离线调整航路点（重新采样并重新规划速度）  
  
  
`在直线时：轨迹加速到最大的速度 在进入弯道时：提前完成减速，曲线曲率越大减速越多 在弯道时：保持恒定速度 在退出弯道时：轨迹开始加速` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4`  
  
相关参数  
•  
[ ] Detail of app tab  
  
  
◦  
[ ] On `multi_lane`, please select multiple input files. If you want lane change with `lane_select`, prepare ver3 type.  
◦  
[ ] Check `replanning_mode` if you want to replan velocity.  
  
  
▪  
[ ] On replanning mode:  
  
  
•  
[ ] Check `realtime_tuning_mode` if you want to tune waypoint.  
•  
[ ] Check `resample_mode` if you want to resample waypoints. On resample mode, please set `resample_interval`.  
•  
[ ] Velocity replanning parameter  
  
  
◦  
[ ] Check `replan curve mode` if you want to decelerate on curve.  
◦  
[ ] Check `overwrite vmax mode` if you want to overwrite velocity of all waypoint.  
◦  
[ ] Check `replan endpoint mode` if you want to decelerate on endpoint.  
◦  
[ ] `Vmax` is max velocity.  
◦  
[ ] `Rth` is radius threshold for extracting curve in waypoints. Increasing this, you can extract curves more sensitively.  
◦  
[ ] `Rmin` and `Vmin` are pairs of values used for velocity replanning. Designed velocity plan that minimizes velocity in the assumed sharpest curve. In the _i_-th curve, the minimum radius _r__i_ and the velocity _v__i_ are expressed by the following expressions. _v__i_ = _Vmax_ - _(Vmax - Vmin)/(Rth - Rmin)_ * _(Rth - r__i__)_  
◦  
[ ] `Accel limit` is acceleration value for limitting velocity.  
◦  
[ ] `Decel limit` is deceleration value for limitting velocity.  
◦  
[ ] `Velocity Offset` is offset amount preceding the velocity plan.  
◦  
[ ] `Braking Distance` is the number of minimum velocity before end point offset.  
◦  
[ ] `End Point Offset` is the number of 0 velocity points at the end of waypoints.  
  
.  
  
.

### **（3）waypoint_saver节点（录制航线生成csv文件）**

功能：当订阅到“/current_pose”、“/current_velocity（选项）”的话题后，将其以指定的时间间隔保存在路点csv文件中。  
  
csv文件以第三种格式保存  
  
  
`ver3：consist of x,y,z,yaw,velocity,change_flag（ category names are on the first line）` `•` `[ ] 1`  
  
`change_flag`为0代表直行，`change_flag`为1代表左转，`change_flag`为2代表右转  
  
相关参数  
  
  
`- ~save_filename - ~interval - ~velocity_topic - ~pose_topic - ~save_velocity` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5`  
  
.  
  
.

### **（4）waypoint_extractor节点(加载录制的航线)**

功能：把录制的航线提取到ros的话题上，一般可以是autoware_msgs/LaneArray  
  
相关参数  
  
  
`- ~lane_csv - ~lane_topic` `•` `[ ] 1` `•` `[ ] 2`  
  
.  
  
.

### **（5）waypoint_creator节点(根据原来加载的路径点进行路径插值、路径平滑)**

功能：使用线性进行插值，或者使用样条函数进行插值 (option: “linear” or “spline”)  
  
注意，这里提供的是一种方法，插值的方法在interpolate.h实现  
  
.  
  
.

### **（6）waypoint_marker_publisher节点**

功能：把traffic light、global waypoints、local waypoints、lane_stop等话题可视化到RVIZ上  
  
.  
  
.

### **（7）waypoint_velocity_visualizer节点**

功能：把路径的速度可视化到RVIZ上  
  
.  
  
.

### **方案二、waypoint_planner（astar做局部规划）**

（A*是负责基于栅格地图的规划探索）

### **（1）规划原理介绍**

功能：航路点上的避让行为决策逻辑、动态速度规划等等 要启动astar进行局部规划，仅仅需要把astar的标志位置为true就可以了（在对应的launch文件中）  
  
  
  
在之前**lane_planner**规划的全局路径**/base_waypoints**的基础上，使用**waypoint_planner**中的astar算法进行局部避障，输出一条安全的路径**/safety_waypoints**  
  
其中，astar_search是算法的实现，astar_avoid是针对astar的一些前置决策  
  
/safety_waypoints可用于车辆控制模块  
  
waypoint_planner功能包对应的节点  
  
  
`astar_avoid astar_search Velocity_set` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3`  
  
.

![[post-images/e167ae1f04d64d73a476f6fc7efd479e.png]]

### **（2）规划代码解析及注释**

【rqt_graph可以查看核对一下的】  
  
我把注释后的代码上传到给github了，从new_avoid_motion_planning.launch看起  
  
这个的方案astar仅仅规划一次，所以无法应对动态障碍物，只要改一改逻辑实现边走边进行astar规划，一直规划就可以实现动态避障的效果

### **（1）astar_avoid节点**

功能：  
  
简单来说就是启动astar算法的前置逻辑决策  
  
这种决策是通过if-else进行的简单逻辑决策，有planningstopreplan等几种状态  
  
根据障碍物obsta的位置确定规划的终点target（可设置多个目标点goal进行尝试），起点可以通过当前定位得到  
  
`astar_avoid`节点有两种模式，中继模式和回避模式。可以通过“enable_avoidation”参数切换这些模式。  
  
-中继模式：不进行避障规划，正常沿着全局路径行驶  
  
-避障模式：通过“astar_search”包中的混合A*搜索算法，通过内部状态转换避开障碍物，如果您的ADAS地图中有“wayarea”，则可以通过在“costmap_generator”节点中启用“Use wayarea”来限制搜索区域并实现更多安全规划，具体可以看看freespace_planner/README.md

### **（2）astar算法的实现astar_search节点**

注意一下，autoware的astar算法版本是根据实际情况优化过的，毕竟图搜索的规划算法都被改烂了～  
  
  
  
.

![[post-images/fc99e0431f484346a149fea391556d13.png]]

### **（3）Velocity_set速度规划模块**

/safety_waypoint是对位置进行了规划，/final_waypoint是对速度也做了规划 velicity_set是为了发布/final_waypoint velicity_set没有对轨迹的位置进行太大的改动，而是针对突发情况下（如人行横道的停车、动态障碍物的减速等等）的轨迹速度进行了改动  
  
  
  
直接用点云的数量来判断是否为障碍物！  
  
.

![[post-images/3089bfcff97d442c8d1f4845a4a103c0.png]]

### **方案三、open_planner**

【局部动态轨迹级规划】  
  
（open_planner是基于语义地图vector_map进行采样的局部规划优化）

![[post-images/8a935bb760ad472ba9aa549768db266d.png]]

### **（1）规划原理流程介绍**

  
  
/lane_waypoints_array是从mission_planner得到的全局路径  
  
**（1）op_common_param**  
  
该节点为open_planner加载公共参数，这些参数由op_trajectory_generator, op_motion_predictor, op_trajectory_evaluator, op_behavior_selector and lidar_kf_contour_track使用  
  
op_local_planner把open_planner的算法参数传进去。如规划距离  
  
**（2）轨迹生成/op_trajectory_generator**  
  
轨迹生成的过程  
  
注意的是，是参考vector_map中的多条/lane_waypoints_array进行生成轨迹的  
  
.  
  
**（3）轨迹代价评分/op_trajectory_Evaluator**  
  
轨迹选优的过程  
  
在open_planner中会根据全局路径生成多个候选路径，每条路径通过代价函数进行评分具有代价，选择代价最低的来走，红色线的是不可行驶的，open_planner是实时运行的

![[post-images/30d4c49ccce74c8388d819c90c489db0.png]]

![[post-images/ec1c7b27c6be4365815b0628e83dfb72.png]]

### **（2）规划代码解析及注释**

open_planner的原理

![[post-images/f560693930e74a549a14e3b2b6a45346.png]]

### **（1）op_common_params（主要）**

### **（2）op_trajectory_evaluator（主要）**

### **（3）op_trajectory_generator（主要）**

### **（4）op_behavior_selector（可选）**

### **（5）op_motion_predictor（可选）**

从new_op_planner.launch看起  
  
.  
  
.

### **方案四、lattice_planner**

### **（1）规划原理流程介绍**

待补充…  
  
.  
  
.

### **（2）规划代码解析及注释**

### **（1）lattice_trajectory_gen（主要）**

### **（2）lattice_twist_convert（主要）**

### **（3）path_select（可选）**

### **（4）lattice_velocity_set（可选）**

.  
  
.

## **三、路径跟踪control**

### **【运动控制算法概述】**

包括路径跟踪控制和VMC的车机控制 . .

![[post-images/d9bdc0ae6bb94cef930e27cc3a06f2b9.png]]

![[post-images/f9fc0d2c58ed46b1bf1e4245bcb4a8d2.png]]

### **方案一、pure_persuit纯跟踪算法**

  
  
autoware实现增加了很多调试和系统话题的接口，移植最好找一个纯净一点的版本  
  
.  
  
.

![[post-images/bbbc2cf6968b4567a6f50b94fb8ad8c8.png]]

### **方案二、模型预测控制mpc算法**

**算法理论**  
  
  
  
**功能包代码解析及注释**  
  
代码其实就是上诉公式的复现，不过加上了很多路径点waypoints的转换、一些flag的逻辑等等  
  
包含在笛卡尔坐标系的横向坐标转换成frenet坐标系的坐标，就是去其横向偏差的过程而已  
  
有两个节点与MPC follower相关。  
  
`/mpc_waypoint_converter`:将`/final_waypoints`转换成 `/mpc_waypoints`，其中包括自身位置后面的航路点。这是为了解决规划系统和mpc跟随者之间的暂时冲突，以便mpc跟随者可以像纯_追求一样使用。这将在未来的版本中删除。  
  
`/mpc_follower`：生成控制命令（`/twist_raw`或/和`/ctrl_raw`）以跟随`/mpc_waypoints`  
  
订阅  
  
  
`- /mpc_waypoints : reference waypoints (generated in mpc_waypoints_converter) - /current_pose : self pose - /vehicle_status : vehicle information (as velocity and steering angle source)` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3`  
  
发布  
  
  
`- /twist_raw : command for vehicle - /ctrl_raw : command for vehicle` `•` `[ ] 1` `•` `[ ] 2`  
  
.  
  
.

![[post-images/c3bf6cb7a43d4b7b8103284bd901b0bc.png]]

![[post-images/d7fe77ed1d5f4db7a9ba9fd953b0f5c3.png]]

![[post-images/04bdb1c43b3249998b60d3e204c462b9.png]]

### **twist_filter控制指令滤波输出**

.  
  
.

## **四、行为决策decision**

### **方案一：state_machine有限状态机**

.  
  
.

### **方案二：decision_make**

功能：  
  
管理车辆状态、给定任务（航路点）状态、行为和运动状态。每个状态都由状态机管理。  
  
.

### **车辆状态VehicleStates**

.

![[post-images/9cf24e161a5a40c99a66526eed6b0db7.jpeg]]

### **全局航线状态MissionStates**

.

![[post-images/cad605f3b2ae4f10b4c5fe2bc376576f.jpeg]]

### **局部航线状态MotionStates**

  
  
.

![[post-images/6677a7ecd02044448b478e55b446e67f.jpeg]]

### **行为和运动状态BehaviorStates**

  
  
.  
  
.

![[post-images/c11300d9e4fe421e9070f474a468d1db.jpeg]]

### **方案三：planner_selector**

在不同场景尝试多种不同局部规划的规划效果

## **机器人/自动驾驶的决策、规划、控制算法工程师的工作要求**

基础的能力  
  
（1）扎实的数学基础，车辆建模能力，数值优化  
  
（2）掌握基本的编程语言和代码编程规范  
  
本职能力  
  
（3）行业的发展现状和各个模块技术情况  
  
（4）【嵌入式】熟悉ARM处理器等嵌入式架构  
  
（5）【地图与定位】栅格地图的设计、拓扑地图的设计、室内外常用的定位方式，与地图、定位模块的对接  
  
（7）【规划】覆盖式路径、全局路径探索、局部路径平滑、曲率优化、轨迹优化  
  
（8）【控制】相关的控制理论的了解，如经典的闭环控制PID理论、现代控制理论、最优控制理论MPC、LQR等  
  
（9）【决策】避障、脱困逻辑的设计  
  
工程能力  
  
（10）代码移植与优化  
  
（11）撰写相关说明文档、技术文档

## **总结**

如果和实现一直进行局部规划–改一改lane_navi逻辑就好了！  
  
规划一般直接用到的是代价地图costmap，还有矢量地图vector_map，但是这两种地图都是从点云地图和高精度度地图中提取出来的

## **参考资料**

综述论文

![[post-images/3ad0986877ac4197acc3e4875de81b4e 2.png|3ad0986877ac4197acc3e4875de81b4e 2.png]]
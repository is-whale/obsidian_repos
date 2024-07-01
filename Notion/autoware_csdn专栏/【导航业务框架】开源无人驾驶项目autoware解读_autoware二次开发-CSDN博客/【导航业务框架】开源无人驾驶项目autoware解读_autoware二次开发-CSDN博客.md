---
Created: 2023-12-18T14:16
Updated: 2023-12-20T17:11
URL: https://blog.csdn.net/qq_35635374/article/details/120893039
---
# **【导航业务框架】开源无人驾驶项目autoware解读**

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/120893039#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/120893039#_13)
- [ ] [一、Autoware的整体框架和模块](https://blog.csdn.net/qq_35635374/article/details/120893039#Autoware_38)
- [ ]
    - [ ] [1.Autoware介绍](https://blog.csdn.net/qq_35635374/article/details/120893039#1Autoware_39)
    - [ ] [2.从多个角度对autoware进行剖析](https://blog.csdn.net/qq_35635374/article/details/120893039#2autoware_68)
    - [ ]
        - [ ] [（1）从Autoware的系统层进行剖析](https://blog.csdn.net/qq_35635374/article/details/120893039#1Autoware_70)
        - [ ] [（2）从系统组件框图进行剖析](https://blog.csdn.net/qq_35635374/article/details/120893039#2_74)
        - [ ] [（3）从算法的基本控制和数据流进行剖析](https://blog.csdn.net/qq_35635374/article/details/120893039#3_78)
        - [ ] [（4）从Autoware的节点图进行剖析【较有用】](https://blog.csdn.net/qq_35635374/article/details/120893039#4Autoware_82)
        - [ ] [（5）从其他图例进行剖析](https://blog.csdn.net/qq_35635374/article/details/120893039#5_87)
    - [ ] [3.Autoware运算部分(核心)](https://blog.csdn.net/qq_35635374/article/details/120893039#3Autoware_93)
    - [ ] [（1）感知部分](https://blog.csdn.net/qq_35635374/article/details/120893039#1_94)
    - [ ]
        - [ ] [（1）目标检测、跟踪模块](https://blog.csdn.net/qq_35635374/article/details/120893039#1_95)
        - [ ]
            - [ ] [Detection检测节点](https://blog.csdn.net/qq_35635374/article/details/120893039#Detection_131)
            - [ ]
                - [ ] [（1）lidar_detector](https://blog.csdn.net/qq_35635374/article/details/120893039#1lidar_detector_132)
                - [ ] [（2）image_detector](https://blog.csdn.net/qq_35635374/article/details/120893039#2image_detector_135)
                - [ ] [（3）image_tracker](https://blog.csdn.net/qq_35635374/article/details/120893039#3image_tracker_138)
                - [ ] [（4）fusion_detector](https://blog.csdn.net/qq_35635374/article/details/120893039#4fusion_detector_141)
                - [ ] [（5）fusion_tools](https://blog.csdn.net/qq_35635374/article/details/120893039#5fusion_tools_144)
                - [ ] [（6）object_tracter](https://blog.csdn.net/qq_35635374/article/details/120893039#6object_tracter_147)
        - [ ] [（2）目标预测模块](https://blog.csdn.net/qq_35635374/article/details/120893039#2_150)
        - [ ]
            - [ ] [目标预测模块节点](https://blog.csdn.net/qq_35635374/article/details/120893039#_152)
            - [ ]
                - [ ] [（1）moving_predictor](https://blog.csdn.net/qq_35635374/article/details/120893039#1moving_predictor_154)
                - [ ] [（2）collision_predictor](https://blog.csdn.net/qq_35635374/article/details/120893039#2collision_predictor_156)
        - [ ] [（3）传感器采集模块、地图构建SLAM模块、地图服务server模块【矢量化】](https://blog.csdn.net/qq_35635374/article/details/120893039#3SLAMserver_160)
        - [ ]
            - [ ] [（1）传感器采集模块](https://blog.csdn.net/qq_35635374/article/details/120893039#1_161)
            - [ ] [（2）地图构建SLAM模块](https://blog.csdn.net/qq_35635374/article/details/120893039#2SLAM_189)
            - [ ] [（3）地图服务server模块【地图矢量化】](https://blog.csdn.net/qq_35635374/article/details/120893039#3server_197)
            - [ ] [（4）地图信息的传递](https://blog.csdn.net/qq_35635374/article/details/120893039#4_335)
            - [ ] [（5）地图数据包获取](https://blog.csdn.net/qq_35635374/article/details/120893039#5_340)
        - [ ] [（4）定位模块](https://blog.csdn.net/qq_35635374/article/details/120893039#4_345)
        - [ ]
            - [ ] [Localization:定位模块节点：](https://blog.csdn.net/qq_35635374/article/details/120893039#Localization_352)
            - [ ]
                - [ ] [（1）lidar_localizar](https://blog.csdn.net/qq_35635374/article/details/120893039#1lidar_localizar_353)
                - [ ] [（2）gnss_localizer](https://blog.csdn.net/qq_35635374/article/details/120893039#2gnss_localizer_356)
                - [ ] [（3）dead_reckoner](https://blog.csdn.net/qq_35635374/article/details/120893039#3dead_reckoner_359)
    - [ ] [（2）规划决策部分](https://blog.csdn.net/qq_35635374/article/details/120893039#2_364)
    - [ ]
        - [ ] [（1）任务规划Misson planning](https://blog.csdn.net/qq_35635374/article/details/120893039#1Misson_planning_372)
        - [ ]
            - [ ] [Misson planning：任务规划节点](https://blog.csdn.net/qq_35635374/article/details/120893039#Misson_planning_376)
            - [ ]
                - [ ] [（1）route_planner](https://blog.csdn.net/qq_35635374/article/details/120893039#1route_planner_377)
                - [ ] [（2）lane_planner](https://blog.csdn.net/qq_35635374/article/details/120893039#2lane_planner_380)
                - [ ] [（3）waypoint_planner](https://blog.csdn.net/qq_35635374/article/details/120893039#3waypoint_planner_383)
                - [ ] [（4）waypoint_maker](https://blog.csdn.net/qq_35635374/article/details/120893039#4waypoint_maker_386)
        - [ ] [（2）运动规划Motion planning](https://blog.csdn.net/qq_35635374/article/details/120893039#2Motion_planning_389)
        - [ ]
            - [ ] [Motion planning:运动规划节点](https://blog.csdn.net/qq_35635374/article/details/120893039#Motion_planning_391)
            - [ ]
                - [ ] [（1）velovity_planner](https://blog.csdn.net/qq_35635374/article/details/120893039#1velovity_planner_392)
                - [ ] [（2）astar_planner](https://blog.csdn.net/qq_35635374/article/details/120893039#2astar_planner_395)
                - [ ] [（3）adas_lattice_planner](https://blog.csdn.net/qq_35635374/article/details/120893039#3adas_lattice_planner_398)
                - [ ] [（4）waypoint_follower](https://blog.csdn.net/qq_35635374/article/details/120893039#4waypoint_follower_401)
                - [ ] [（5）open_planner](https://blog.csdn.net/qq_35635374/article/details/120893039#5open_planner_404)
                - [ ] [......](https://blog.csdn.net/qq_35635374/article/details/120893039#_407)
        - [ ] [（3）行为规划Behavioral Planning](https://blog.csdn.net/qq_35635374/article/details/120893039#3Behavioral_Planning_409)
    - [ ] [4.Autoware控制部分【Path following路径跟踪】](https://blog.csdn.net/qq_35635374/article/details/120893039#4AutowarePath_following_415)
    - [ ]
        - [ ] [（1）介绍](https://blog.csdn.net/qq_35635374/article/details/120893039#1_416)
        - [ ] [（2）算法类型](https://blog.csdn.net/qq_35635374/article/details/120893039#2_422)
        - [ ]
            - [ ] [（1）PID控制器](https://blog.csdn.net/qq_35635374/article/details/120893039#1PID_423)
            - [ ] [（2）pure pursuit纯跟踪算法](https://blog.csdn.net/qq_35635374/article/details/120893039#2pure_pursuit_427)
            - [ ] [（3）MPC算法](https://blog.csdn.net/qq_35635374/article/details/120893039#3MPC_430)
    - [ ] [5.Autoware运动仿真Wf_simulation](https://blog.csdn.net/qq_35635374/article/details/120893039#5AutowareWf_simulation_434)
- [ ] [二、autoware的源码解读](https://blog.csdn.net/qq_35635374/article/details/120893039#autoware_441)
- [ ]
    - [ ] [1.Autoware源码下载及解读方法](https://blog.csdn.net/qq_35635374/article/details/120893039#1Autoware_442)
    - [ ] [2.Autoware源码的具体功能包解读【按照功能分类】](https://blog.csdn.net/qq_35635374/article/details/120893039#2Autoware_460)
    - [ ]
        - [ ] [（1）根据功能包的功能实现来解读的步骤](https://blog.csdn.net/qq_35635374/article/details/120893039#1_461)
        - [ ] [（2）根据不同功能包实现的功能进行对应功能包的解读】【学习某个原理并验证】](https://blog.csdn.net/qq_35635374/article/details/120893039#2_467)
        - [ ]
            - [ ] [(1)Map 地图](https://blog.csdn.net/qq_35635374/article/details/120893039#1Map__469)
            - [ ] [(2)Sensing 传感器](https://blog.csdn.net/qq_35635374/article/details/120893039#2Sensing__470)
            - [ ] [(3)Localization 定位](https://blog.csdn.net/qq_35635374/article/details/120893039#3Localization__471)
            - [ ] [(4)Detection 目标检测](https://blog.csdn.net/qq_35635374/article/details/120893039#4Detection__472)
            - [ ] [(5)Mission Planning 任务规划](https://blog.csdn.net/qq_35635374/article/details/120893039#5Mission_Planning__473)
            - [ ] [(6)Motion Planning 运动规划](https://blog.csdn.net/qq_35635374/article/details/120893039#6Motion_Planning__474)
        - [ ] [（3）Autoware导航文件架构解读](https://blog.csdn.net/qq_35635374/article/details/120893039#3Autoware_481)
- [ ] [三、autoware实操教程](https://blog.csdn.net/qq_35635374/article/details/120893039#autoware_486)
- [ ]
    - [ ] [1.autoware官方git教程](https://blog.csdn.net/qq_35635374/article/details/120893039#1autowaregit_490)
    - [ ]
        - [ ] [（1）官网资料（科普补理论）](https://blog.csdn.net/qq_35635374/article/details/120893039#1_493)
    - [ ] [2.Users Guide用户教程步骤（按照官方的方式试试水）](https://blog.csdn.net/qq_35635374/article/details/120893039#2Users_Guide_514)
    - [ ] [3.搭建自己的无人驾驶项目field test步骤](https://blog.csdn.net/qq_35635374/article/details/120893039#3field_test_519)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/120893039#_536)
- [ ]
    - [ ] [部署经验总结](https://blog.csdn.net/qq_35635374/article/details/120893039#_548)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/120893039#_568)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！

在项目和平时的学习中，我对**机器人/无人驾驶的决策规划模块**进行了划分，当然划分的方法有很多，我的划分方式仅供参考 **（1）动态障碍物行为预测模块（Behavior prediction）**–结合感知和高精度地图信息，估计周围障碍物未来运动状态 **（2）执行机构的轨迹规划模块（Trajectory_planning）**–执行机构如机器人载体上的[机械臂](https://so.csdn.net/so/search?q=%E6%9C%BA%E6%A2%B0%E8%87%82&spm=1001.2101.3001.7020)、串行云台等的运动轨迹规划 **（3）任务决策模块（Mission_planning）**–任务决策模块比较偏业务层了，处理机器人/[无人驾驶](https://so.csdn.net/so/search?q=%E6%97%A0%E4%BA%BA%E9%A9%BE%E9%A9%B6&spm=1001.2101.3001.7020)的各种任务，主要分为三个方面：车底盘航线业务决策（交规、横向换道等等）、执行应用机构业务决策（机械臂、人机交互等等）、不同场景的导航方案切换决策（组合导航、融合导航） **（4）前端路径探索模块（path_finding）**–全局路径规划算法难度不算复杂，找到一条可通行的（必须满足）、考虑动力学的（尽可能满足）、可以是稀疏的路径base_waypoints【由于其只考虑了环境几何信息，往往忽略了无人机本身的运动学与动力学模型。因此，其得到的轨迹往往显得比较“突兀”，并不适合直接作为无人机的控制指令】 **（5）后端轨迹处理模块（motion_planning）**–我主要归纳整理为三个方向：（1）对base_waypoints进行简单处理及生成方向、（2）对base_waypoints轨迹优化方向【一般是二次优化，这里用的较多的事优化方面的知识】、（3）进行对应功能的replan方向（replan之前的预处理、进入replan的条件、停障replan、避障replan、纠偏replan、换道replan、自动泊车replan、穿过狭窄道路replan等等），这部分内容使用的方法比较专 **（6）路径跟踪模块（trajectory_following）**–这个模块就得针对机器人载体了，如无人驾驶使用得阿克曼模型可以采用几何的pure pursuit纯追踪算法，更好的可以用模型预测控制MPC方法，还有强化学习做的（效果怎样我就没验证过了）；当然也可能事麦克纳姆轮车、差速车PID、还有无人机的三维轨迹跟踪等等 **（7）碰撞检测模块** **（8）集群多机器人规划模块**

当然，这种划分方式是我权衡了原理和功能粗略划分的，在实际产品研发过程中，需要理解了各个算法的功能和定位的基础上融汇贯通，不能生搬硬套，如全段通过hybrid A_探索出来的路径与A_、RRT*探索出来的路径更平滑，后端轨迹优化的任务就不用这么重了；又如，机器人/无人驾驶项目研发的需求业务还没发展到能响应很多功能阶段，任务决策使用简单的状态机（fsm）就可以对现有任务进行状态转移了

本文先对**开源无人驾驶项目autoware解读**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章

---

## **一、Autoware的整体框架和模块**

### **1.Autoware介绍**

Autoware是世界上第⼀款⽤于⾃动驾驶汽⻋的“⼀体化”和“多合一”开源软件。**Autoware的功能主要适⽤于城市，但⾼速公路和⾮市政道路同样可以使⽤**。【防盗标记–盒子君hzj】同时在依赖ROS建⽴的Autoware⾃动驾驶开源软件上可以提供丰富的开发和使⽤资源  
  
**Autoware支持以下功能:**  
  
（1）传感器校准  
  
（2）传感器融合  
  
（3）各种数据记录  
  
（4）3D本地化  
  
（5）3D映射  
  
（6）面向云的地图连接自动化  
  
（7）汽车/行人/物体检测  
  
（8）交通信号检测  
  
（9）车道检测  
  
（10）对象跟踪  
  
（11）路径规划  
  
（12）智能手机导航  
  
（13）加速/制动/转向的路径跟踪控制  
  
（14）软件仿真  
  
（15）虚拟现实  
  
**整个autoware有很多种算法集成在一起**，没有一键启动的说法，先按照自己的需求配置好相应功能的启动项，再启动

### **2.从多个角度对autoware进行剖析**

每个模块采用它自己的一套算法

### **（1）从Autoware的系统层进行剖析**

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 11.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 11.]]

### **（2）从系统组件框图进行剖析**

这个框架少了高精度矢量地图的部分【SLAM建图部分–静态地图及矢量化，其他地图图层生成】

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 1 4.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 1 4.]]

### **（3）从算法的基本控制和数据流进行剖析**

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_182Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_182Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2.]]

### **（4）从Autoware的节点图进行剖析【较有用】**

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2 4.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2 4.]]

### **（5）从其他图例进行剖析**

. .

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 3 3.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 3 3.]]

### **3.Autoware运算部分(核心)**

### **（1）感知部分**

### **（1）目标检测、跟踪模块**

**（1）介绍**  
自动驾驶车辆必须检测周围物体，如其他车辆、行人和行人交通信号灯。有了这些信息，我们能遵守交通规则，避免事故发生。检测模块使用摄像头和激光雷达，结合传感器融合算法和深度学习网络进行目标检测。目标识别与检测，基于先进的YOLOv3检测模型，实时地检测车辆前方环境以及障碍，实现自主停障和在监视屏上显示当前行驶的状态  
  
**（2）关键技术**  
1）视觉全景图像融合技术  
将独立的图像拼接成无缝360度的环视图  
2）视觉目标检测识别技术  
从图像数据中准确阅读文本信息、识别交通标识、识别动态人车信息  
3）点云数据配准技术  
点云畸变的纠偏  
4）点云障碍物检测技术  
从激光雷达中的点云数据中识别检测出行人、车辆障碍物等等  
  
**（3）目标检测算法**  
探测物体，比如车辆，行人和交通信号灯，以避免碰撞和违反交通规则。【防盗标记–盒子君hzj】我们专注于在移动物体（车辆和行人）上，虽然我们的平台也能识别交通信号灯。我们使用可变形零件模型（DPM）算法进行检测车辆和行人。  
CUDA是NVIDIA开发的一个编程框架，用于通用计算在GPU  
  
**（4）目标跟踪算法**  
我们解决这个跟踪问题使用了两种算法：  
卡尔曼滤波是在线性系统下使用的假设我们的自动驾驶汽车跟踪时匀速行驶它的计算量是重量轻，适合实时处理  
粒子过滤器可以适用于非线性跟踪场景我们的自动驾驶汽车和履带车辆正在移动。  
我们使用卡尔曼滤波器和粒子滤波器，这取决于给定场景。我们还将它们应用于二维（图像）平面和三维平面上的跟踪三维（点云）平面  

### **Detection检测节点**

### **（1）lidar_detector**

从激光雷达单帧扫描读取点云信息，提供基于激光雷达的目标检测。【防盗标记–盒子君hzj】主要使用欧几里德聚类算法，从地面以上的点云得到聚类结果。除此之外，可以使用基于卷积神经网路的算法进行分类，包括VoxelNet,LMNet.

### **（2）image_detector**

读取来自摄像头的图片，提供基于图像的目标检测。主要的算法包括R-CNN，SSD和Yolo，可以进行多类别(汽车，行人等)实时目标检测。

### **（3）image_tracker**

使用image_detector的检测结果完成目标跟踪功能。算法基于Beyond Pixels，图像上的目标跟踪结果被投影到3D空间，结合lidar_detector的检测结果输出最终的目标跟踪结果。

### **（4）fusion_detector**

输入激光雷达的单帧扫描点云和摄像头的图片信息，进行在3D空间的更准确的目标检测。激光雷达的位置和摄像头的位置需要提前进行联合标定，现在主要是基于MV3D算法来实现。

### **（5）fusion_tools**

将lidar_detector和image_detector的检测结果进行融合，image_detector 的识别类别被添加到lidar_detector的聚类结果上。

### **（6）object_tracter**

预测检测目标的下一步位置，跟踪的结果可以被进一步用于目标行为分析和目标速度分析【防盗标记–盒子君hzj】。跟踪算法主要是基于卡尔曼滤波器。

### **（2）目标预测模块**

自动驾驶汽车的安全是一个高度优先的问题。因此，感知模块必须计算出“自我”飞行器在三维空间中的准确位置映射，并识别周围场景中的对象作为交通信号灯的状态。预测模块使用定位和检测的结果来预测跟踪目标。通过卡尔曼滤波算法和3D高精度地图实现，基于概率机器人技术和基于规则的系统，部分还使用深度神经网络

### **目标预测模块节点**

### **（1）moving_predictor**

使用目标跟踪的结果来预测临近物体的未来行动轨迹，例如汽车或者行人

### **（2）collision_predictor**

使用moving_predictor的结果来进一步预测未来是否会与跟踪目标发生碰撞【防盗标记–盒子君hzj】。输入的信息包括车辆的跟踪轨迹，车辆的速度信息和目标跟踪信息

### **（3）传感器采集模块、地图构建SLAM模块、地图服务server模块【矢量化】**

### **（1）传感器采集模块**

传感器和计算机的规格在很大程度上取决于功能自动驾驶要求  
  
_（1）摄像头（Camera）信息_  
  
全方位覆盖360度视野，用来检测运动物体，用来检测运动物体识别红绿灯  
  
_（2）激光雷达（lidar）信息_  
  
Velodyne激光雷达传感器、Ibeo激光雷达传感器、Hokuyo激光雷达传感器、【防盗标记–盒子君hzj】毫米波雷达和惯性测量装置通常是自动飞行器的首选  
  
摄像机和激光雷达传感器通常在10到100赫兹的频率下运行，每个任务自动驾驶必须在一定条件下进行时间限制  
  
点云库（PCL）是主要用于管理激光雷达扫描和三维地图数据  
  
_（3）惯导系统（IMU）信息_  
  
_（4）定位系统（GNSS）信息_  
  
定位系统RTK传感器信息、定位系统gnss传感器信息  
  
接收来自卫星的全球定位信息，通常与陀螺IMU传感器和里程表相结合，以确定位置信息。  
  
**传感器采集方案**  
  
采集、建图、处理数据部分是有一辆数据采集车车独立执行的  
  
（1）低成本移动激光测量进行数据采集的方案 在小车等移动载体上采集得到激光雷达高精度点云数据、甚至是相机数据、GPS/IMU航迹里程计数据  
  
  
  
（2）一体化高精度地图SLAM的解决方案

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 4 2.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 4 2.]]

![[post-images/154121c935264eb0bd65bbd22524338c 2.png|154121c935264eb0bd65bbd22524338c 2.png]]

### **（2）地图构建SLAM模块**

通过在每次扫描时注册3D点云数据，创建并更新3D地图。这通常被称为SLAM，【防盗标记–盒子君hzj】基于实时定位与建图（SLAM）技术，把道路上的路标、障碍物等信息数字化，精确的描绘出来

高精度地图的输出格式是怎样的？ [https://github.com/ApolloAuto/apollo/blob/master/modules/map/data/README.md](https://github.com/ApolloAuto/apollo/blob/master/modules/map/data/README.md) 参考README 所示，xml, bin, txt, lb1 都是不同的文件格式，适配不同的读取器，内容是一致的

### **（3）地图服务server模块【地图矢量化】**

（1）矢量地图图例  
  
  
  
（2）矢量地图的定义  
  
矢量数据结构是通过记录坐标的方式,尽可能精确地表示点线多边形等地理实体,自然地理实体的位置是用其在坐标参考系中的空间位置来定义的,坐标空间设为连续,允许任意位置长度和面积的精确定栅格图是由组成图形的大量像素点来确定，栅格结构表示的是不连续的,离散的数据,其最明显的特点是属性明显,定位隐含  
  
矢量地图也称为高清地图，矢量地图包含自主导航模块需要用来理解周围环境的数据，【防盗标记–盒子君hzj】矢量地图或道路网络地图包含的所有离散信息，例如交通信号灯，交通标志，十字路口，停车线等的位置  
  
（3）矢量地图的业务场景  
  
（无人驾驶场景）车道线路径规划+巡航+道路级的任务规划（换道，返航，停障等等）  
  
规划算法按重要性排序的矢量地图组件及在自动驾驶系统中的用途  
  
  
  
（4）地图矢量化过程（基于人工绘制矢量地图）  
  
将栅格地图或其他格式的地图数据转换为矢量地图数据的过程叫做地图矢量化，【防盗标记–盒子君hzj】通过arcmap或者mapinfo等工具，将栅格数据的地图路网和POI数据进行矢量化绘制，以不同图层的形式加载显示，地图矢量化以后精度更高，路网拓扑更清晰，数据量也更小，缩放不会失真。  
  
将栅格数据或者其他格式的数据通过一定的方式转换为矢量数据的过程就叫矢量化。【防盗标记–盒子君hzj】通俗点可以这样理解，在一张地图图片上，描绘出点线面文件…  
  
基于先验完全信息的算法【防盗标记–盒子君hzj】，在高精度地图中绘制出矢量地图，矢量地图包含了人工设定的多条可行路径和交通规则（使用autowere的方法）  
  
步骤如下：  
  
（1）使用点云制图软件制作高精度地图  
  
（2）手动方式制作矢量地图的工具  
  
工具一：vector map builder的使用  
  
工具二：ztlidar的使用  
  
（5）Autoware ADAS Map矢量地图的元素介绍  
  
1、介绍  
  
Autoware ADAS Map的矢量地图的格式是Vector map格式，Vector map中定义的地图元素多样，每种元素的属性字段各异，各个元素之间通过特殊字段链接起来，且元素均采用.csv格式独立保存  
  
用于储存矢量地图所需的要素，【防盗标记–盒子君hzj】Autoware将激光点云数据进行分类和下采样完成后，把所有的点按照其定义的类型重新组织，其中point代表点云中某点，Autoware对每个点进行唯一编号，包含经纬高blh和平面xy坐标。之后根据分类好的元素，按照line、vector、area分别组织数据  
  
2、元素分类  
  
定义于src/autoware/core_planning/lane_planner/include/lane_planner/lane_planner_vmap.hpp。用于储存决策规划所需要的部分矢量地图要素数据  
  
  
`struct` `VectorMap` `{` `std` `::``vector``<``vector_map``::``Point``>` `points``;` `std` `::``vector``<``vector_map``::``Lane``>` `lanes``;` `std` `::``vector``<``vector_map``::``Node``>` `nodes``;` `std` `::``vector``<``vector_map``::``StopLine``>` `stoplines``;` `std` `::``vector``<``vector_map``::``DTLane``>` `dtlanes``;``}``;` `•` `[ ] 1` `•` `[ ] 2` `•` `[ ] 3` `•` `[ ] 4` `•` `[ ] 5` `•` `[ ] 6` `•` `[ ] 7` `•` `[ ] 8`  
  
Autoware将激光点云数据进行分类和下采样完成后，把所有的点按照其定义的类型重新组织  
  
1）vector_map::Point【位于src/autoware/messages/vector_map_msgs/msg/Point.msg】  
  
2）vector_map::Lane【位于src/autoware/messages/vector_map_msgs/msg/Lane.msg】  
  
3）vector_map::Node【位于src/autoware/messages/vector_map_msgs/msg/Node.msg】  
  
4）vector_map::StopLine【位于src/autoware/messages/vector_map_msgs/msg/StopLine.msg】  
  
5）vector_map::DTLane【位于src/autoware/messages/vector_map_msgs/msg/Line.msg】  
  
**1、点元素（point.csvnode.csvdtlane.csv）**  
  
**（1）点元素（point.csv）格式说明**  
  
point.csv中存储了地图中所有点元素的几何位置属性，包括路径节点node、路径特征补充节点dtlane、线状面状端点  
  
格式说明如下： point点的属性如下： 1、通常point点相隔1米分布 2、point点为属性变更点或者多条lane规划线的链接节点 **（2）点元素（node.csv）格式说明** Node是地图生成车道lane的元素，是车道lane元素的节点，【防盗标记–盒子君hzj】node节点相连形成了车道lane。在绘制地图时，绘制一个node后会在point.csv中存储几何及位置信息，node.csv与point.csv通过PID相链接 **（3）点元素（dtlane.csv）格式说明** Dtlane是对车道元素lane的几个形状特征的补充元素，在绘制地图时，【防盗标记–盒子君hzj】绘制一个Dtlane后会在point.csv中存储几何及位置信息，node.csv与point.csv通过PID相链接  
  
  
  
**2、车道元素（lane.csv）**  
  
**（1）车道元素（lane.csv）格式说明** lane是车道元素，地图中的lane代表小车形式的轨迹路径，一般是道路的中心线，在显示道路中是没有的，是靠人为规划出来的 其中，node为路径lane的节点，【防盗标记–盒子君hzj】所以每段lane的长度即是before node和forward node之间的距离 lane代表了地图中所有可通行的路径，在绘制时要把所有可以通行的情况绘制清楚  
  
  
  
**3、线元素（line.csvwhiteline.csvstopline.csv）**  
  
（1）线元素（line.csv）格式说明  
  
line.csv储存了所有线元素的基本属性 （2）线元素（whiteline.csv）格式说明 Whiteline是车道线，即是真是存在的线，【防盗标记–盒子君hzj】如道路上的白线、黄线、双黄线、虚白线（导流线）等等  
  
  
  
**4、杆元素**  
  
**5、面元素（area.csv/crosswalk.csv/road_surface_mark.csv）**  
  
（1）面元素（area.csv）格式说明 area储存了所有的面元素的基本属性，area由点point和线line组成，端点信息储存在point.csv中，边线信息储存在line.csv中 其中，面元素包括：人行横道区域（crosswalk.csv）、【防盗标记–盒子君hzj】斑马线区域（zebrazone.csv）、道路标志（road surface_mark.csv）、下水井盖区域（gutter.csv）  
  
  
  
格式说明如下： （2）面元素（crosswalk.csv）格式说明 格式说明如下： （3）面元素（road surface_mark.csv）格式说明 格式说明如下：  
  
  
  
3、元素关系图例 （1）点元素和lane元素的关系 Point、node、dtlane、lane是矢量地图vector的四个必要元素，缺一不可。使用该四种元素就可以构建一个最简单的高精度地图，把地体导入autoware系统中进而实现路径规划仿真极真车操作但由于元素比较少，对于道路的停止线、红绿灯、等更复杂的路面无法满足自动驾驶的操作，还需要更多的元素  
  
  
  
**（6）地图用于规划**  
  
（1）全局地图的建立（用于路径巡航）  
  
三维雷达通过SLAM建立三维稠密点云全局地图，【防盗标记–盒子君hzj】通过arcmap或者mapinfo等工具，将栅格数据的地图路网、POI数据和三维稠密点云地图进行矢量化绘制在矢量地图，矢量地图提供相关.csv文件给全局规划用，一般用于巡航

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 5 2.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 5 2.]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 6]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_122Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

![[post-images/3b556184ce7a4f4c9aa6696c21d4579f.png]]

![[post-images/c1191f44987448e19ccd8b2cf66076d9.png]]

![[post-images/7b82b1ddaefe453bbdebd8616992f9ef.png]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_112Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 7]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 8]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 9]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_142Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

![[post-images/34416034b68244408c355fb91147190b.png]]

![[post-images/34276d4f20cd43adb60a99b9361e71cc.png]]

![[post-images/0118a44aa0c744ad955f738b322b4dd8.png]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_112Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 1]]

![[post-images/9386e972ec9e4d798c896526c2e45507.png]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 10]]

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 11]]

（2）局部地图的建立（用于[轨迹规划](https://so.csdn.net/so/search?q=%E8%BD%A8%E8%BF%B9%E8%A7%84%E5%88%92&spm=1001.2101.3001.7020)）

三维雷达建立稠密点云局部地图，局部点云地图生成八叉树代价地图给三维局部规划，点云地图压缩成二维点云地图进而生成栅格代价地图给局部规划，一般用于道路级的任务规划（换道，返航，停障，避障等等）

---

**（7）把地图上传到服务器，进行共享调用**

### **（4）地图信息的传递**

Lane.csv文件经过多层处理得到的结果是约1米的waypoint,waypoint包含位置和速度两个信息

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 12]]

### **（5）地图数据包获取**

方法一：Rosbag store中有很多传感器数据集需要付费进行使用，复杂开发的时候要给钱了  
  
方法二：自己搭ros的仿真环境，获取传感器数据

### **（4）定位模块**

定位模块使用SLAM算法、3D map(高精度地图)服务、NDT来实现，【防盗标记–盒子君hzj】使用从CAN消息和GNSS/IMU传感器获取的里程计信息，通过Kalman滤波算法对定位结果进行补充  
  
Autoware主要使用NDT算法进行局部化。这是因为无损检测的计算成本算法不受地图大小的支配，从而使高清晰度和高分辨率3D的部署大比例尺地图  
  
Autoware还支持其他本地化和映射算法，如迭代最近点（ICP）算法。这允许Autoware用户为其应用程序选择最适合的算法

### **Localization:定位模块节点：**

### **（1）lidar_localizar**

计算车辆当在全局坐标的当前位置(x,y,z,roll,pitch,yaw)，使用LIDAR的扫描数据和预先构建的地图信息。autoware推荐使用正态分布变换(NDT)算法来匹配激光雷达当前帧和3D map

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 13]]

### **（2）gnss_localizer**

转换GNSS接收器发来的NEMA消息到位置信息(x,y,z,roll,pitch,yaw)。结果可以被单独使用为车辆当前位置，也可以作为lidar_localizar的初始参考位置

### **（3）dead_reckoner**

主要使用IMU传感器预测车辆的下一帧位置，【防盗标记–盒子君hzj】也可以用来对lidar_localizar和gnss_localizar的结果进行插值。

### **（2）规划决策部分**

autoware的**规划任务计划是半自主的**，在更复杂的场景中，例如停车和恢复工作错误，司机需要监督路径  
  
规划是**基于概率机器人技术**和**基于场景规则的系统**根据交通规则，autoware使用**基于场景规则的自主分配路径的机制**，比如换道，合并，前进  
  
根据预先构建的高精地图，**基于概率机器人技术**规划一条从起点到终点的、安全的、且光滑的轨迹。车辆基于规划路线完成自主巡逻任务

### **（1）任务规划Misson planning**

在全局中，指定一个起点，一个终点，通过全局规划得到一条全局静态路径，一旦分配了全局路径，本地马上启动运动规划器

### **Misson planning：任务规划节点**

### **（1）route_planner**

寻找到达目标地点的全局路径并**发布的一系列十字路口结果**，路径由道路route网中的一系列十字路口组成。

### **（2）lane_planner**

根据route_planner发布的一系列十字路口结果，**确定全局路径由哪些lane组成**，lane是由一系列waypoint点组成

### **（3）waypoint_planner**

用于**产生到达目的地的一系列waypont点**，【防盗标记–盒子君hzj】它与lane_planner的不同之处在于它是发布单一的到达目的地的waypoint路径,而lane_planner是发布到达目的地的一系列waypoint数组

### **（4）waypoint_maker**

waypoint_maker 是一个**保存和加载手动制作的waypoint文件的工具**。为了保存waypoint到文件里，需要手动驾驶车辆并开启定位模块，然后记录车辆的一系列定位信息以及速度信息， 被记录的信息汇总成为一个路径文件，之后可以加载这个本地文件，并发布需要跟踪的轨迹路径信息给其他规划模块

### **（2）运动规划Motion planning**

planning负责根据给定的一段temporal waypoint轨迹生成局部可行轨迹全局轨迹，考虑到车辆状态，三维地图所示的位置及其局部的障碍物分布情况，周围对象、交通规则和期望的目标，在局部环境下，规划出来一条最优的路径

### **Motion planning:运动规划节点**

### **（1）velovity_planner**

更新车辆速度信息，注意到给定跟踪的waypoint里面是带有速度信息的，这个模块就是根据车辆的实际状态进一步修正速度信息，以便于实现在停止线前面停止下来或者加减速等等

### **（2）astar_planner**

实现Hybrid-State A*查找算法，生成从现在位置到指定位置的可行轨迹，【防盗标记–盒子君hzj】这个模块可以实现避障，或者在给定waypoint下的急转弯，也包括在自由空间内的自动停车

### **（3）adas_lattice_planner**

实现了State Lattice规划算法，事先定义好的参数列表和语义地图信息，基于mini_jerk样条曲线实现局部路径生成与优化，在当前位置前方产生了多条可行路径，最终得到无人车执行的最终轨迹final waypoint，可以被用来进行障碍物避障或车道线换道

### **（4）waypoint_follower**

这个模块实现了 Pure Pursuit算法来实现轨迹跟踪，可以产生一系列的控制指令来移动车辆，这个模块发出的控制消息可以被车辆控制模块订阅，或者被线控接口订阅，最终就可以实现车辆自动控制

### **（5）open_planner**

原理和实现写在我其他博客

### **…**

### **（3）行为规划Behavioral Planning**

Autoware实现了一个智能状态机、做出决策以响应道路状况。  
  
通过深度学习的方法，根据驾驶状态，判断行车过程中的行为决策，【防盗标记–盒子君hzj】例如当车道变换、合并和超车

### **4.Autoware控制部分【Path following路径跟踪】**

### **（1）介绍**

控制车辆跟随运动规划器生成的路径  
  
Autoware已安装并通过许多有线控车辆进行了测试。Autoware的计算输出是一组速度、角速度、轮角和曲率。这些信息通过车辆接口作为命令发送给有线控制器。**控制转向和油门需要由线控器来控制。经常采用PID**

### **（2）算法类型**

### **（1）PID控制器**

一般用于线控底盘的油门、刹车和转向的控制  
  
通常采用PID控制器、在某些参数不正确的情况下，PID控制器将无法控制车辆稳定。

### **（2）pure pursuit纯跟踪算法**

这个方法将路径分解为多个航路点是路径的离散表示。【防盗标记–盒子君hzj】每次控制期间循环，算法搜索下一个最近的航路点在汽车的前进方向。航路点是在临界距离下搜索。这减少了突然的转弯或跟车时可能发生的角度变化偏离路线

### **（3）MPC算法**

在另外的博客写

### **5.Autoware运动仿真Wf_simulation**

（1）介绍  
  
wf_simulation相当于一个虚拟的小车提供了一个仿真的汽车坐标base_link,能够响应速度转向指令，并反馈base_link的当前位置，姿态、速度  
  
（2）操作步骤 （3）autoware有两个仿真环境，【防盗标记–盒子君hzj】一个是gazebo，一个是LGSVL

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 14]]

## **二、autoware的源码解读**

### **1.Autoware源码下载及解读方法**

自己根据官方的源码已经能学到很多东西了，根据界面操作和launch文件逻辑，结合节点图去分析程序  
  
Autoware源码可以去官网的git下载进行阅读学习  
  
链接统一放在文章末尾的参考资料中  
  
根据程序的架构来解读的方法（自己的经验而已）  
  
步骤一：先按照教程把效果跑出来  
  
步骤二：阅读注释源码  
  
步骤三：根据工程的架构有一个整体的理解  
  
步骤四：学习函数的功能和输入输出接口、在学习函数的实现过程  
  
步骤五：根据百度谷歌来理解原理  
  
步骤六：移植到自己的工程架构中，加深工程部署的经验  
  
.  
  
.  
  
.

### **2.Autoware源码的具体功能包解读【按照功能分类】**

### **（1）根据功能包的功能实现来解读的步骤**

1、先看main函数，（1）设置的参数、（2）发布的话题、（3）订阅的话题  
  
2、分析订阅的回调函数的代码逻辑  
  
3、得到节点实现的功能

### **（2）根据不同功能包实现的功能进行对应功能包的解读】【学习某个原理并验证】**

根据autoware界面上功能的操作进行代码解读

### **(1)Map 地图**

### **(2)Sensing 传感器**

### **(3)Localization 定位**

### **(4)Detection 目标检测**

### **(5)Mission Planning 任务规划**

### **(6)Motion Planning 运动规划**

这个涉及原理，在本文科普中不详细分析，功能包太多了，我尽量挑我用过的在其他博客中分享出来，先战略转移，毕竟又要10点下班了  
  
.  
  
.  
  
.

### **（3）Autoware导航文件架构解读**

这相当于autoware各个功能包的分类，我是理解成一个导航架构，按着这个结构和功能我会把具体每个功能包的原理在其他博客写一些吧 先放个总图哈

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 15]]

## **三、autoware实操教程**

目的：  
  
【主要式发掘autoware有什么功能】  
  
【快速搭建出效果评价-看源码实现、把源码移植到自己的工程上】

### **1.autoware官方git教程**

这里的步骤教程其实在autoware的官方git已经描述的很详细了，还有油管视频，这个就简单吧步骤捋一捋…

### **（1）官网资料（科普补理论）**

（1）Autoware的官网 [https://github.com/Autoware-AI/autoware.ai](https://github.com/Autoware-AI/autoware.ai) [https://www.autoware.ai](https://www.autoware.ai/)

（2）sample rosbag files

这个用tizi下载比较快

（3）Autoware Wiki介绍

[https://github.com/Autoware-AI/autoware.ai/wiki](https://github.com/Autoware-AI/autoware.ai/wiki)

（4）Autoware-Manuals

[https://github.com/CPFL/Autoware-Manuals](https://github.com/CPFL/Autoware-Manuals)

（5）论文

S. Kato, S. Tokunaga, Y. Maruyama, S. Maeda, M. Hirabayashi, Y. Kitsukawa, A. Monrroy, T. Ando, Y. Fujii, and T. Azumi,``Autoware on Board: Enabling Autonomous Vehicles with Embedded Systems,‘’ In Proceedings of the 9th ACM/IEEE International Conference on Cyber-Physical Systems (ICCPS2018), pp. 287-296, 2018. Link

S. Kato, E. Takeuchi, Y. Ishiguro, Y. Ninomiya, K. Takeda, and T. Hamada. ``An Open Approach to Autonomous Vehicles,‘’ IEEE Micro, Vol. 35, No. 6, pp. 60-69, 2015. Link.

.

.

### **2.Users Guide用户教程步骤（按照官方的方式试试水）**

我这样表达步骤可能更清晰

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 16]]

### **3.搭建自己的无人驾驶项目field test步骤**

我这样表达步骤可能更清晰（自己的经验，仅供参考） . . .

![[post-images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 17]]

## **总结**

Autoware仅仅是一个工具，实现的源码内容还是得靠自己的去学，学习源码原理参考百度，Autoware的整体是比较复杂的，整体解读清楚它的原理短时间是不可能实现的，所以别整体的源码去看，从自己需要的模块逐个突破  
  
感知部分：包含动态障碍物的检测、跟踪和预测，包括交通灯的检测和分类  
  
定位部分：GNSS定位、NDT定位  
  
地图部分：高精度地图  
  
规划决策部分：全局路线加载，车道场景、倒车泊车场景  
  
控制部分：纯跟踪算法、MPC算法  
  
车机接口部分：can接口

### **部署经验总结**

（1）autoware的bag数据包得改成自己得做验证，有实物传感器更好啦，没有的化自己搭gazebo仿真一样能拿到传感器数据 （2）autoware在ubuntu18.04也可以装1.12以上的版本【防盗标记–盒子君hzj】 （3）安装autoware从简单装起来，用到什么装什么 （4）装好autoware启动界面肯定亏出现花屏的现象，按照博客来pip装wx，再改文件就行 [https://www.cxybb.com/article/m0_46673077/115400045](https://www.cxybb.com/article/m0_46673077/115400045) [https://codeleading.com/article/10816031861/](https://codeleading.com/article/10816031861/)

（5）使用bag_demo运行起来的时候仅仅只有一个CPU在运行是正常的，后面用自己的bag数据包自己选择算法的时候CPU运行就可选了

.

**跑autoware的方法**

autoware在整体编译不过的情况下，把对应的功能包移植出来运行

直接运行相关的demo launch就行（这个github本来就是有点问题！），阅读源码为主，整体运行不起来没问题的

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
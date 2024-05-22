---
Created: 2023-12-18T14:11
Updated: 2023-12-18T14:11
URL: https://blog.csdn.net/qq_35635374/article/details/120591633
---
# **【感知的prediction】无人车行为预测**

![[images/original 15.png|original 15.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newUpTime2 4.png|newUpTime2 4.png]]

已于 2022-05-02 11:54:01 修改

![[images/articleReadEyes2 15.png|articleReadEyes2 15.png]]

阅读量533

收藏  
  
  
4  

![[images/tobarCollect2 15.png|tobarCollect2 15.png]]

![[images/newHeart2023Black 15.png|newHeart2023Black 15.png]]

点赞数  
2  

分类专栏：[==# 感知perception==](https://blog.csdn.net/qq_35635374/category_11464719.html)[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)[==# 无人驾驶相关apollo==](https://blog.csdn.net/qq_35635374/category_11523332.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**同时被 3 个专栏收录**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 14.|resize2Cm_fixed2Ch_2242Cw_224 14.]]

![[images/newArrowDown1White 3.png|newArrowDown1White 3.png]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 14.png|studyVipIcon 14.png]]

[**感知perception**](https://blog.csdn.net/qq_35635374/category_11464719.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 1 3.|resize2Cm_fixed2Ch_2242Cw_224 1 3.]]

8 篇文章  
4 订阅  

==订阅专栏==

[**无人驾驶相关apollo**](https://blog.csdn.net/qq_35635374/category_11523332.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 2 3.|resize2Cm_fixed2Ch_2242Cw_224 2 3.]]

3 篇文章  
2 订阅  

==订阅专栏==

提示：文章写完后，目录可以自动生成，如何生成可参考右边的帮助文档

### **机器人/无人驾驶的导航决策规划（planning分组）**

### **文章目录**

- [ ]
    - [ ] [机器人/无人驾驶的导航决策规划（planning分组）](https://blog.csdn.net/qq_35635374/article/details/120591633#planning_3)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/120591633#_11)
- [ ] [一、导航中预测未知空间的占据情况方法](https://blog.csdn.net/qq_35635374/article/details/120591633#_35)
- [ ]
    - [ ] [1.一种基于深度神经网络的方法，可以可靠地预测未知空间的占据情况](https://blog.csdn.net/qq_35635374/article/details/120591633#1_36)
    - [ ] [2.论文参考](https://blog.csdn.net/qq_35635374/article/details/120591633#2_38)
    - [ ] [3.参考网站](https://blog.csdn.net/qq_35635374/article/details/120591633#3_44)
    - [ ] [4.参考代码](https://blog.csdn.net/qq_35635374/article/details/120591633#4_48)
- [ ] [二、动态障碍物行为预测介绍](https://blog.csdn.net/qq_35635374/article/details/120591633#_55)
- [ ]
    - [ ] [1.不同的预测方式](https://blog.csdn.net/qq_35635374/article/details/120591633#1_58)
    - [ ] [2.预测目标车道](https://blog.csdn.net/qq_35635374/article/details/120591633#2_65)
    - [ ] [3.预测障碍物状态](https://blog.csdn.net/qq_35635374/article/details/120591633#3_68)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/120591633#_73)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
在项目和平时的学习中，我对**机器人/无人驾驶的决策规划模块**进行了划分，当然划分的方法有很多，我的划分方式仅供参考  
  
**（1）动态障碍物行为预测模块（Behavior prediction）**–结合感知和高精度地图信息，估计周围障碍物未来运动状态  
  
**（2）执行机构的轨迹规划模块（Trajectory_planning）**–执行机构如机器人载体上的机械臂、串行云台等的运动轨迹规划  
  
**（3）任务决策模块（Mission_planning）**–任务决策模块比较偏业务层了，处理机器人/无人驾驶的各种任务，主要分为三个方面：车底盘航线业务决策（交规、横向换道等等）、执行应用机构业务决策（机械臂、人机交互等等）、不同场景的导航方案切换决策（组合导航、融合导航）  
  
**（4）前端路径探索模块（path_finding）**–全局路径规划算法难度不算复杂，找到一条可通行的（必须满足）、考虑动力学的（尽可能满足）、可以是稀疏的路径base_waypoints【由于其只考虑了环境几何信息，往往忽略了无人机本身的运动学与动力学模型。因此，其得到的轨迹往往显得比较“突兀”，并不适合直接作为无人机的控制指令】  
  
（5）**后端轨迹处理模块（motion_planning）**–我主要归纳整理为三个方向：（1）对base_waypoints进行简单处理及生成方向、（2）对base_waypoints轨迹优化方向【一般是二次优化，这里用的较多的事优化方面的知识】、（3）进行对应功能的replan方向（replan之前的预处理、进入replan的条件、停障replan、避障replan、纠偏replan、换道replan、自动泊车replan、穿过狭窄道路replan等等），这部分内容使用的方法比较专  
  
（6）**路径跟踪模块（trajectory_following）**–这个模块就得针对机器人载体了，如无人驾驶使用得阿克曼模型可以采用几何的pure pursuit纯追踪算法，更好的可以用模型预测控制MPC方法，还有强化学习做的（效果怎样我就没验证过了）；当然也可能事麦克纳姆轮车、差速车PID、还有无人机的三维轨迹跟踪等等  
  
**（7）碰撞检测模块**  
  
**（8）集群多机器人规划模块**  
  
当然，这种划分方式是我权衡了原理和功能粗略划分的，在实际产品研发过程中，需要理解了各个算法的功能和定位的基础上融汇贯通，不能生搬硬套，如全段通过hybrid A_探索出来的路径与A_、RRT*探索出来的路径更平滑，后端轨迹优化的任务就不用这么重了；又如，机器人/无人驾驶项目研发的需求业务还没发展到能响应很多功能阶段，任务决策使用简单的状态机（fsm）就可以对现有任务进行状态转移了  
  
本文先对**动态障碍物行为预测模块（Behavior prediction）**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章提示：以下是本篇文章正文内容

## **一、导航中预测未知空间的占据情况方法**

### **1.一种基于深度**[**神经网络**](https://so.csdn.net/so/search?q=%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C&spm=1001.2101.3001.7020)**的方法，可以可靠地预测未知空间的占据情况**

.

### **2.论文参考**

Learning-based 3D Occupancy Prediction for Autonomous Navigation in Occluded Environments  
  
https://arxiv.org/abs/2011.03981v1  
  
.

### **3.参考网站**

https://blog.csdn.net/Travis_X/article/details/115680240  
  
.

### **4.参考代码**

https://github.com/ZJU-FAST-Lab/OPNet  
  
.  
  

**二、动态障碍物行为预测介绍**结合感知和高精度地图信息，估计周围障碍物未来运动状态预测模块能够学习新的行为，可以使用多源的数据进行训练，使算法随时间的推移而提升预测能力  
  
.**1.不同的预测方式**（1）基于模型的预测  
  
模型预测根据候选模型模拟自己的运行轨迹，【防盗标记–盒子君hzj】通过车辆的移动观测它与那条轨迹更加匹配，优点在于它的直观并且结合物理知识、交通法规、人类行为等多方面  
  
（2）基于数据驱动预测  
  
数据驱动预测基于机器学习，通过观测结果来训练模型，模型一旦训练好可以通过模型去做预测。数据驱动方法优点是训练数据越多，效果更好**2.预测目标车道**apollo为建立车道序列，先将道路分成多部分，【防盗标记–盒子君hzj】每一部分覆盖了一个易于描述车辆运动的区域，将车辆行为划分为一组有限的模式组合，并将这些模式组合描述成车道序列,使用车道序列框架的目的是生成轨迹**3.预测障碍物状态**预测模块会预测从物体到车道线段边界的纵向和横向距离，而且还包含之前时间间隔的状态信息，以便做出更准确的预测**总结** 本文仅仅是简单介绍了预测模块的一些方向，【防盗标记--盒子君hzj】具体的后续整理好资料再更吧，在研究生毕业之际，通过博客记录下自己的成长过程，认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！

   
  
显示推荐内容
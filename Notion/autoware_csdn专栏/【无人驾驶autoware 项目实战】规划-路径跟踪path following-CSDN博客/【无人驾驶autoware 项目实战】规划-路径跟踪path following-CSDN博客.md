---
Created: 2023-12-18T14:13
Updated: 2023-12-18T14:13
URL: https://blog.csdn.net/qq_35635374/article/details/121860283
---
# **【无人驾驶autoware 项目实战】规划-路径跟踪path following**

![[images/original 14.png|original 14.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newCurrentTime2 11.png|newCurrentTime2 11.png]]

于 2021-12-10 16:54:38 发布

![[images/articleReadEyes2 14.png|articleReadEyes2 14.png]]

阅读量1.2k

收藏  
  
  
7  

![[images/tobarCollect2 14.png|tobarCollect2 14.png]]

![[images/newHeart2023Black 14.png|newHeart2023Black 14.png]]

点赞数  
4  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 13.|resize2Cm_fixed2Ch_2242Cw_224 13.]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 13.png|studyVipIcon 13.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121860283#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121860283#_13)
- [ ] [一、功能](https://blog.csdn.net/qq_35635374/article/details/121860283#_28)
- [ ] [二、输入](https://blog.csdn.net/qq_35635374/article/details/121860283#_34)
- [ ]
    - [ ] [【final waypoint】](https://blog.csdn.net/qq_35635374/article/details/121860283#final_waypoint_35)
- [ ] [三、输出](https://blog.csdn.net/qq_35635374/article/details/121860283#_41)
- [ ]
    - [ ] [【twist_command】](https://blog.csdn.net/qq_35635374/article/details/121860283#twist_command_42)
- [ ] [四、算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121860283#_49)
- [ ]
    - [ ] [【waypoint following】](https://blog.csdn.net/qq_35635374/article/details/121860283#waypoint_following_50)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121860283#_63)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121860283#_71)

---

## **前言**

  
  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**规划-路径跟踪path following**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 9.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 9.]]

## **一、功能**

车辆跟踪运动规划motion planning输出的可执行轨迹final_waypoint  
  
.  
  
.

## **二、输入**

### **【final waypoint】**

无人车执行的最终轨迹  
  
.  
  
.

## **三、输出**

### **【twist_command】**

最终的线速度target speed、角速度angular speed  
  
.  
  
.

## **四、算法流程实现**

### **【waypoint following】**

常用的是PID算法、纯跟踪pure_persuit算法了、模型预测控制MPC算法、LQR算法，这个模块实现了 Pure Pursuit算法来实现轨迹跟踪，可以产生一系列的控制指令来移动车辆，这个模块发出的控制消息可以被车辆控制模块订阅，或者被线控接口订阅，最终就可以实现车辆自动控制

相关一系列算法我写在了其他博客，转战一下： [【control】pure pursuit纯追踪算法](https://blog.csdn.net/qq_35635374/article/details/120704494) [【无人驾驶路径跟踪控制】无人驾驶路径跟踪控制常用方法](https://blog.csdn.net/qq_35635374/article/details/121762304) [【control】特定场景PID路径巡线控制器](https://blog.csdn.net/qq_35635374/article/details/120707004)

---

## **总结**

## **参考资料**

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
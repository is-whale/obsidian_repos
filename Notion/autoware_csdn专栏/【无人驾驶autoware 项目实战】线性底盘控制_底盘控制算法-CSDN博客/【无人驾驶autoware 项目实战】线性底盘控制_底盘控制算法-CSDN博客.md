---
Created: 2023-12-18T14:13
Updated: 2023-12-18T14:13
URL: https://blog.csdn.net/qq_35635374/article/details/121860370
---
# **【无人驾驶autoware 项目实战】线性底盘控制**

![[images/original 18.png|original 18.png]]

[盒子君~](https://blog.csdn.net/qq_35635374)

![[images/newCurrentTime2 14.png|newCurrentTime2 14.png]]

于 2021-12-10 16:56:56 发布

![[images/articleReadEyes2 18.png|articleReadEyes2 18.png]]

阅读量1.6k

收藏  
  
  
9  

![[images/tobarCollect2 18.png|tobarCollect2 18.png]]

![[images/newHeart2023Black 18.png|newHeart2023Black 18.png]]

点赞数  
5  

分类专栏：[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)文章标签：[==自动驾驶==](https://so.csdn.net/so/search/s.do?q=%E8%87%AA%E5%8A%A8%E9%A9%BE%E9%A9%B6&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)[==机器学习==](https://so.csdn.net/so/search/s.do?q=%E6%9C%BA%E5%99%A8%E5%AD%A6%E4%B9%A0&t=all&o=vip&s=&l=&f=&viparticle=)

版权

[**无人驾驶相关Autoware  
  
**](https://blog.csdn.net/qq_35635374/category_11523328.html)[**专栏收录该内容**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

![[images/resize2Cm_fixed2Ch_2242Cw_224 17.|resize2Cm_fixed2Ch_2242Cw_224 17.]]

25 篇文章  
38 订阅  
  
  
**¥9.90**~~¥99.00~~

==已订阅==超级会员免费看

![[images/studyVipIcon 17.png|studyVipIcon 17.png]]

## **系列文章目录**

提示：这里可以添加系列文章的所有文章的目录，目录需要自己手动添加 TODO:写完再整理

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121860370#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121860370#_13)
- [ ] [一、功能](https://blog.csdn.net/qq_35635374/article/details/121860370#_25)
- [ ] [二、输入](https://blog.csdn.net/qq_35635374/article/details/121860370#_32)
- [ ]
    - [ ] [【twist_command】](https://blog.csdn.net/qq_35635374/article/details/121860370#twist_command_33)
- [ ] [三、输出](https://blog.csdn.net/qq_35635374/article/details/121860370#_39)
- [ ] [四、算法流程实现](https://blog.csdn.net/qq_35635374/article/details/121860370#_46)
- [ ]
    - [ ] [【twist_filtering】](https://blog.csdn.net/qq_35635374/article/details/121860370#twist_filtering_47)
    - [ ] [【阿克曼线性底盘的PID解算】](https://blog.csdn.net/qq_35635374/article/details/121860370#PID_56)
- [ ] [总结](https://blog.csdn.net/qq_35635374/article/details/121860370#_65)
- [ ] [参考资料](https://blog.csdn.net/qq_35635374/article/details/121860370#_73)

---

## **前言**

  
  
认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**线性底盘控制**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

## **一、功能**

接收path following输出的车辆线控twist指令，根据车的运动学、动力学模型和机械参数，输出车辆分别控制轮子的控制量  
  
.  
  
.

## **二、输入**

### **【twist_command】**

最终的线速度target speed、角速度angular speed  
  
.  
  
.

## **三、输出**

【vehicle control】  
  
车辆分别控制轮子的控制量  
  
.  
  
.

## **四、算法流程实现**

### **【twist_filtering】**

twist控制指令滤波，输出更平滑的twist控制指令

滤波算法原理我写在另外的博客，请转战一下~ [https://blog.csdn.net/qq_35635374/article/details/120936564](https://blog.csdn.net/qq_35635374/article/details/120936564)

.

.

### **【阿克曼**[**线性**](https://so.csdn.net/so/search?q=%E7%BA%BF%E6%80%A7&spm=1001.2101.3001.7020)**底盘的PID解算】**

根据车的运动学、动力学模型和机械参数，输出车辆分别控制轮子的控制量

算法原理我写在另外的博客，请转战一下~

[无人车电控（线控技术）底盘研发](https://blog.csdn.net/qq_35635374/article/details/121180713)

---

## **总结**

这里的仅仅包括控制算法相关的知识，不包含底盘ECU等嵌入式设计实现

## **参考资料**

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[OpenCV技能树](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/opencv/?utm_source=csdn_ai_skill_tree_blog)_23318_ _人正在系统学习中_

 _显示推荐内容_
---
Created: 2023-12-18T14:12
Updated: 2023-12-18T14:12
URL: https://blog.csdn.net/qq_35635374/article/details/121021698
---
# **【传感器sensor】机器人/无人驾驶常用传感器模型、选型与安装**

分类专栏：[==# 感知perception==](https://blog.csdn.net/qq_35635374/category_11464719.html)[==# 无人驾驶相关Autoware==](https://blog.csdn.net/qq_35635374/category_11523328.html)[==# 无人驾驶相关apollo==](https://blog.csdn.net/qq_35635374/category_11523332.html)文章标签：[==算法==](https://so.csdn.net/so/search/s.do?q=%E7%AE%97%E6%B3%95&t=all&o=vip&s=&l=&f=&viparticle=)[==人工智能==](https://so.csdn.net/so/search/s.do?q=%E4%BA%BA%E5%B7%A5%E6%99%BA%E8%83%BD&t=all&o=vip&s=&l=&f=&viparticle=)

[**无人驾驶相关Autoware  **](https://blog.csdn.net/qq_35635374/category_11523328.html)[**同时被 3 个专栏收录**](https://blog.csdn.net/qq_35635374/category_11523328.html)[](https://blog.csdn.net/qq_35635374/category_11523328.html)

[**感知perception**](https://blog.csdn.net/qq_35635374/category_11464719.html)

[**无人驾驶相关apollo**](https://blog.csdn.net/qq_35635374/category_11523332.html)

### **文章目录**

- [ ] [系列文章目录](https://blog.csdn.net/qq_35635374/article/details/121021698#_0)
- [ ] [前言](https://blog.csdn.net/qq_35635374/article/details/121021698#_13)
- [ ] [一、常用传感器类型及模型特性介绍【核心】](https://blog.csdn.net/qq_35635374/article/details/121021698#_24)
- [ ]
    - [ ] [1.轮式编码器（提供里程计）](https://blog.csdn.net/qq_35635374/article/details/121021698#1_25)
    - [ ]
        - [ ] [编码器内参标定方法](https://blog.csdn.net/qq_35635374/article/details/121021698#_30)
    - [ ] [2.IMU惯性导航系统（提供里程计、位姿）](https://blog.csdn.net/qq_35635374/article/details/121021698#2IMU_36)
    - [ ]
        - [ ] [1. 惯性技术简介](https://blog.csdn.net/qq_35635374/article/details/121021698#1__37)
        - [ ] [2. 惯性器件误差分析](https://blog.csdn.net/qq_35635374/article/details/121021698#2__68)
        - [ ] [3. 惯性器件内参标定](https://blog.csdn.net/qq_35635374/article/details/121021698#3__104)
        - [ ] [4. 惯性器件温补](https://blog.csdn.net/qq_35635374/article/details/121021698#4__122)
        - [ ] [5. 惯性导航解算](https://blog.csdn.net/qq_35635374/article/details/121021698#5__127)
    - [ ] [3.雷达Lidar（提供点云）](https://blog.csdn.net/qq_35635374/article/details/121021698#3Lidar_134)
    - [ ]
        - [ ] [（1）线激光雷达Lidar](https://blog.csdn.net/qq_35635374/article/details/121021698#1Lidar_135)
        - [ ]
            - [ ] [1）激光雷达介绍](https://blog.csdn.net/qq_35635374/article/details/121021698#1_136)
            - [ ] [2）激光雷达(Lidar)的硬件连接方式](https://blog.csdn.net/qq_35635374/article/details/121021698#2Lidar_154)
            - [ ] [3）激光雷达(Lidar)优缺点](https://blog.csdn.net/qq_35635374/article/details/121021698#3Lidar_160)
        - [ ] [（2）固态激光雷达Lidar](https://blog.csdn.net/qq_35635374/article/details/121021698#2Lidar_172)
        - [ ] [（3）毫米波雷达Radar](https://blog.csdn.net/qq_35635374/article/details/121021698#3Radar_183)
        - [ ] [（4）雷达标定](https://blog.csdn.net/qq_35635374/article/details/121021698#4_199)
        - [ ]
            - [ ] [1. 雷达和相机外参标定](https://blog.csdn.net/qq_35635374/article/details/121021698#1__200)
            - [ ] [2. 雷达内参标定](https://blog.csdn.net/qq_35635374/article/details/121021698#2__210)
            - [ ] [3. 多雷达外参标定](https://blog.csdn.net/qq_35635374/article/details/121021698#3__221)
    - [ ] [4.相机Camera（提供图像、点云）](https://blog.csdn.net/qq_35635374/article/details/121021698#4Camera_229)
    - [ ]
        - [ ] [（1）视觉传感器介绍](https://blog.csdn.net/qq_35635374/article/details/121021698#1_231)
        - [ ] [（2）常用相机观测模型](https://blog.csdn.net/qq_35635374/article/details/121021698#2_240)
        - [ ]
            - [ ] [1）针孔相机观测模型](https://blog.csdn.net/qq_35635374/article/details/121021698#1_241)
            - [ ] [2）双目相机观测模型](https://blog.csdn.net/qq_35635374/article/details/121021698#2_283)
            - [ ] [3）RGB-D相机观测模型](https://blog.csdn.net/qq_35635374/article/details/121021698#3RGBD_299)
        - [ ] [（3）视觉传感器的优缺点](https://blog.csdn.net/qq_35635374/article/details/121021698#3_310)
    - [ ] [5.超声波Ultrosonic（提供距离信息）](https://blog.csdn.net/qq_35635374/article/details/121021698#5Ultrosonic_320)
    - [ ] [6.红外传感器（提供距离信息）](https://blog.csdn.net/qq_35635374/article/details/121021698#6_335)
    - [ ] [7.触觉传感器（提供距离信息）](https://blog.csdn.net/qq_35635374/article/details/121021698#7_340)
- [ ] [二、传感器选型](https://blog.csdn.net/qq_35635374/article/details/121021698#_346)
- [ ] [三、传感器的安装](https://blog.csdn.net/qq_35635374/article/details/121021698#_351)
- [ ] [四、传感器的一般标定类型](https://blog.csdn.net/qq_35635374/article/details/121021698#_355)
- [ ]
    - [ ] [1.内参标定](https://blog.csdn.net/qq_35635374/article/details/121021698#1_356)
    - [ ] [2.外参标定](https://blog.csdn.net/qq_35635374/article/details/121021698#2_360)
    - [ ] [3.时间标定](https://blog.csdn.net/qq_35635374/article/details/121021698#3_364)
- [ ] [五、传感器融合](https://blog.csdn.net/qq_35635374/article/details/121021698#_384)
- [ ]
    - [ ] [卡尔曼融合案例](https://blog.csdn.net/qq_35635374/article/details/121021698#_385)

---

## **前言**

认知有限，望大家多多包涵，有什么问题也希望能够与大家多交流，共同成长！  
  
本文先对**机器人/无人驾驶常用传感器模型、选型与安装**做个简单的介绍，具体内容后续再更，其他模块可以参考去我其他文章  
  
提示：以下是本篇文章正文内容

## **一、常用传感器类型及模型特性介绍【核心】**

### **1.轮式编码器（提供里程计）**

分为位置式**绝对型编码器**、**增量式编码器**  
  
打比赛的时候用过欧姆龙的增量式编码器，也用过待编码器的步进电机。【防盗标记–盒子君hzj】增量式编码器正交编码输出信号，用于识别正转和反转，通过微控制器（单片机）的片内外设硬件/软件解码

### **编码器内参标定方法**

1）目的：用编码器输出解算车的位移增量和角度增量，需已知轮子半径和两轮轴距 2） 方法：以车中心雷达/RTK做观测，以此为真值，反推模型参数 3）论文： Simultaneous Calibration of Odometry and [Sensor](https://so.csdn.net/so/search?q=Sensor&spm=1001.2101.3001.7020) Parameters for Mobile Robots . .

### **2.IMU惯性导航系统（提供里程计、位姿）**

### **1. 惯性技术简介**

**（1）惯性器件-机械陀螺**  
  
定轴性：当陀螺转子以高速旋转时，在没有任何外力矩作用在陀螺仪上时，【防盗标记–盒子君hzj】陀螺仪的自转轴在惯性空间中的指向保持稳定不变，即指向一个固定的方向；同时反抗任何改变转子轴向的力量  
  
进动性：当转子高速旋转时，若外力矩作用于外环轴，陀螺仪将绕内环轴转动；【防盗标记–盒子君hzj】若外力矩作用于内环轴，陀螺仪将绕外环轴转动。其转动角速度方向与外力矩作用方向互相垂直  
  
**（2）惯性器件-激光陀螺**  
  
Sagnac效应由法国物理学家 Sagnac 于 1913 年发现，其原理是干涉仪相对惯性空间静止时, 光路A 和 B 的光程相等，当有角速度时，光程不相等，便会产生干涉  
  
**（3）惯性器件-光纤陀螺**  
  
同样基于Sagnac效应，传播介质改成了光纤  
  
**（4）惯性器件-MEMS陀螺**  
  
科里奥利力（Coriolis force，简称为科氏力），是对旋转体系（【防盗标记–盒子君hzj】比如自传的地球，旋转的圆盘等）中进行直线运动的质点由于惯性相对于旋转体系产生的直线运动的偏移的一种描述  
  
**（5）惯性器件-加速度计**  
  
当运载体相对惯性空间做加速度运动时，仪表壳体也随之做相对运动，质量块保持惯性，朝着与加速度方向相反的方向产生位移（拉伸或压缩弹簧）。当位移量达到一定值时，弹簧给出的力使质量块以同一加速度相对惯性空间做加速运动，加速度的大小与方向影响质量块相对位移的方向及拉伸量  
  
.  
  
.

### **2. 惯性器件**[**误差分析**](https://so.csdn.net/so/search?q=%E8%AF%AF%E5%B7%AE%E5%88%86%E6%9E%90&spm=1001.2101.3001.7020)

---

**1) 量化噪声**

一切量化操作所固有的噪声，是数字传感器必然出现的噪声；

产生原因：通过AD采集把连续时间信号采集成离散信号的过程中，【防盗标记–盒子君hzj】精度会损失，精度损失的大小和AD转换的步长有关，步长越小，量化噪声越小

---

**2) 角度随机游走**

宽带角速率白噪声：陀螺输出角速率是含噪声的，该噪声中的白噪声成分，角度误差中所含的马尔可夫性质的误差，称为角度随机游走

产生原因：计算姿态的本质是对角速率做积分，这必然会对噪声也做了积分。白噪声的积分并不是白噪声，而是一个马尔可夫过程，即当前时刻的误差是在上一时刻误差的基础上累加一个随机白噪声得到的

---

**3) 角速率随机游走**

与角度随机游走类似，角速率误差中所含的马尔可夫性质的误差，【防盗标记–盒子君hzj】称为角速率随机游走。而这个马尔可夫性质的误差是由宽带角加速率白噪声累积的结果

---

**4) 零偏不稳定性噪声**

零偏:即常说的bias，一般不是一个固定参数，而是在一定范围内缓慢随机飘移

零偏不稳定性:零偏随时间缓慢变化，其变化值无法预估，需要假定一个概率区间描述它有多大的可能性落在这个区间内。时间越长，区间越大

---

**5) 速率斜坡**

该误差是趋势性误差，而不是随机误差。随机误差，是指你无法用确定性模型去拟合并消除它，最多只能用概率模型去描述它，这样得到的预测结果也是概率性质的，趋势性误差，是可以直接拟合消除的，在陀螺里产生这种误差最常见的原因是温度引起零位变化，可以通过温补来消除

---

**6) 零偏重复性**

多次启动时，零偏不相等，因此会有一个重复性误差。在实际使用中，【防盗标记–盒子君hzj】需要每次上电都重新估计一次，Allan方差分析时，不包含对零偏重复性的分析

.

.

### **3. 惯性器件内参标定**

由于加工原因，产生零偏、标度因数误差、安装误差。  
  
**标定方法**  
  
1)惯性器件内参误差标定  
  
2)分立级标定  
  
3)半系统级标定  
  
4)系统级标定  
  
**不同类型标定方法的比较**  
1.  
[ ] 分立级标定精度较高，但依赖于转台；  
2.  
[ ] 半系统级标定精度最差，但不依赖转台，成本低、效率高，对MEMS的标定需求已经足够；  
3.  
[ ] 系统级标定精度最高，但只适用于标定高精度惯导  
  
**参考**  
  
论文：A Robust and Easy to Implement Method for IMU Calibration without External Equipments  
  
代码：https://github.com/Kyle-ak/imu_tk  
  
.  
  
.

### **4. 惯性器件温补**

温补的本质是系统辨识，既要找出合适的物理模型，又要识别物理模型的参数  
  
温补在器件误差补偿中是最重要的【防盗标记–盒子君hzj】，但也是最“没有技术含量”的  
  
.  
  
.

### **5. 惯性导航解算**

惯性导航解算包括：**姿态解算**、**速度解算**、**位置解算**，其中姿态解算最为核心  
  
姿态解算方法的推导，是整个惯性技术中最为复杂的，但是却最为“无用”的  
  
.  
  
.

### **3.雷达Lidar（提供**[**点云**](https://so.csdn.net/so/search?q=%E7%82%B9%E4%BA%91&spm=1001.2101.3001.7020)**）**

### **（1）线激光雷达Lidar**

### **1）激光雷达介绍**

**（1）介绍** 激光雷达传感器创建环境的点云表征，提供摄像头难以获取的距离或者高度信息。激光点云可以提高物体许多信息，比如其形状和表面纹理，通过对点进行聚类和分析，能通过对象检测、跟踪或分类信息  
  
  
  
一般是多个激光束同时发射，并绕固定轴旋转，来探测三维环境  
  
**（2）激光雷达的工作方式**  
  
激光雷达传感器向周围环境发射脉冲光波；【防盗标记–盒子君hzj】  
  
这些脉冲碰撞到周围物体反弹并返回传感器；  
  
传感器使用每个脉冲返回到传感器所花费的时间来计算其传播的距离；  
  
每秒重复数百万次此过程，将创建精确的实时3D环境地图

![[b45bcc9bf65b4ebc91291763a7a11fb0.png]]

**（3）Velodyne 16线三维激光雷达介绍** [https://blog.csdn.net/Travis_X/article/details/104109095](https://blog.csdn.net/Travis_X/article/details/104109095) . .

### **2）激光雷达(Lidar)的硬件连接方式**

单片机处理：先接到单片机上做处理的，在上传到CPU上的（更好的硬件时间同步）  
  
CPU处理：直接连接pc  
  
.  
  
.

### **3）激光雷达(Lidar)优缺点**

1)优点  
  
测距精度非常高，可以达到厘米级别；  
  
自带光源，不受白天夜晚的环境条件限制  
  
2)缺点  
  
远处的激光点之间的间隔较大，64线扫描距离比较远的物体时会比较稀疏，无法识别远处的物体；  
  
激光雷达想要发射的更远发射的功率就越大，【防盗标记–盒子君hzj】否则会在空间中衰减掉，但由于出于激光对人眼安全的考虑，它有一个功率限制，因此感知范围有限，大概六七十米左右  
  
.  
  
.

### **（2）固态激光雷达Lidar**

非重复扫描，他的扫描方式是梅花瓣状的，经过多次扫描后获得比机械旋转雷达更稠密的点云  
  
.  
  
.

### **（3）毫米波雷达Radar**

与激光雷达类似，但是发射的不是激光而是毫米波，【防盗标记–盒子君hzj】是主动式的感知，不受光照的影响，可以通过多普勒效应测汽车与障碍物的相对速度  
  
**(1)优点**  
  
测距测速都比较准  
  
**(2)缺点**  
  
噪点比较多  
  
对非金属材质的召回比较低  
  
非常稀疏，无法做识别

### **（4）雷达标定**

### **1. 雷达和相机外参标定**

- [ ] 目的：解算雷达和相机之间的相对旋转和平移方法：PnP是主流，视觉提取特征点，雷达提取边缘，建立几何约束参考 论文： LiDAR-Camera Calibration using 3D-3D Point correspondences 代码：https://github.com/ankitdhall/lidar_camera_calibration 论文： Automatic Extrinsic Calibration for Lidar-Stereo Vehicle Sensor Setups 代码： https://github.com/beltransen/velo2cam_calibration

### **2. 雷达内参标定**

- [ ] 目的：由于安装原因，线束之间的夹角和设计不一致，会导致测量不准。方法：多线束打在平面上，利用共面约束，求解夹角误差参考 论文： Calibration of a rotating multi-beam Lidar 论文： Improving the Intrinsic Calibration of a Velodyne LiDAR Sensor 论文： 3D LIDAR–camera intrinsic and extrinsic calibration: Identifiability and analytical least-squares-based initialization

### **3. 多雷达外参标定**

- [ ] 目的：多雷达是常见方案，使用时将点云直接拼接，但前提是已知雷达之间的外参（相对旋转和平移）方法：基于特征(共面)建立几何约束，从而优化外参参考： 论文： A Novel Dual-Lidar Calibration Algorithm Using Planar Surfaces 代码： https://github.com/ram-lab/lidar_appearance_calibration . .

### **4.相机Camera（提供图像、点云）**

### **（1）视觉传感器介绍**

图像在计算机中以矩阵形式存储（二维数组），图像在计算机中以矩阵形式存储（二维数组）  
  
  
  
摄像头图像是最常见的计算机视觉数据，图像中的每一个像素只是一个值，这些值构成图像矩阵，可以改变像素的值，比如添加一个标量整数改变图像亮度。  
  
彩色图像被构建为值的三维立方体，每个立方体都有高度、宽度和深度，深度为颜色通道数，RGB图像深度为3。  
  
.  
  
.

![[60c8a13ed7ab4e618915bb2fcbb42bfd.png]]

### **（2）常用相机观测模型**

### **1）针孔相机观测模型**

**（1）科普**  
  
普通相机可以用针孔模型很好地近似，照片记录了真实世界在成像平面上的投影，但是这个过程丢弃了“距离”维度上的信息  
  
**（2）针孔相机模型示意图** . .  
  
  
  
**（3）针孔相机模型表达式** . .  
  
  
  
**（4）针孔相机投影顺序**  
  
投影顺序：世界——相机——归一化平面——像素 . . **（5）针孔相机畸变类型及模型** （1）畸变类型 1）径向畸变 2）切向畸变 （2）畸变数学模型 畸变可以用归一化坐标的变换来描述 1）径向畸变多项式描述 2）切向畸变多项式描述 3）径向+切向畸变多项式描述 实际当中可灵活保留各项系数 . . **（6）针孔相机观测总步骤**

![[images/watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 4.|watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 4.]]

![[9fb0479d124249db9ef4a801fe061607.png]]

![[watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 1]]

![[watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 2]]

![[281e3a3e0be648fea8863e80dad386bd.png]]

![[013afc7cbdff484fb5ef909548cd6c4f.png]]

![[903ef2c8cc8148cebef6e0aa3c1c3992.png]]

![[6303b08991f84c7088de7e9d29319884.png]]

![[6849e316d4404668be9e9673e6609f2d.png]]

![[9639d64b641547688db8fecf25f2c2ad.png]]

### **2）双目相机观测模型**

s示意图 基线:左右相机中心距离称为基线  
  
  
  
视差（disparity）:d称为视差（disparity），描述同一个点在左右目上成像的距离，d最小为1个像素，因此双目能测量的z有最大值：fb，虽然距离公式简单，但d不容易计算  
  
左右像素的几何关系

![[watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 3]]

![[54492370cdb64330b51b8658914446c2.png]]

### **3）RGB-D相机观测模型**

使用物理手段测量深度，通常能得到与RGB图对应的深度图  
  
（1）飞行时间ToF原理  
  
  
  
（2）结构光原理

![[watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_132Ccolor_FFFFFF2Ct_702Cg_se2Cx_16]]

![[8e412c7444724ab9947833ab043db48f.png]]

### **（3）视觉传感器的优缺点**

（1）优点  
  
比较稠密的感知，基本能看清图像中的细节；  
  
可以感知的范围比较远，通过配置不同的焦距可以看到几百米以外的物体  
  
（2）缺点  
  
易受光线环境的影响；  
  
由于是单目相机，无法获知深度信息  
  
.  
  
.

### **5.超声波Ultrosonic（提供距离信息）**

（1）超声波传感器原理  
  
淘宝10块钱的店铺应该有图  
  
在发送端echo发出信号，在接收端接收返回来的信号，【防盗标记–盒子君hzj】计算发送和接收之间的时间差，距离=（声速*发送和接收之间的时间差）/2  
  
（2）优点  
  
近距离测距  
  
（3）缺点  
  
感知范围有限；  
  
只能在低速环境中做感知  
  
.  
  
.

### **6.红外传感器（提供距离信息）**

原理和超声波相似，只是载体不是声音是红外光，不用计算发送接收的时间差，以模拟量的方式一直运行，通过运放把接收回来的信号放大给到单片机做识别，顾测量距离的远近可以通过调节灵敏电阻实现  
  
.  
  
.

### **7.触觉传感器（提供距离信息）**

我当然是用普通的按键啦，【防盗标记–盒子君hzj】触觉传感器返回的应该也是一个开关信号量而已怎么识别触摸我再深入了解一下  
  
.  
  
.

## **二、传感器选型**

![[watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 4]]

## **三、传感器的安装**

![[watermark2Ctype_ZHJvaWRzYW5zZmFsbGJhY2s2Cshadow_502Ctext_Q1NETiBA55uS5a2Q5ZCbfg3D3D2Csize_202Ccolor_FFFFFF2Ct_702Cg_se2Cx_16 5]]

## **四、传感器的一般标定类型**

### **1.内参标定**

**传感器自身性质**，例如camera焦距，Lidar中各激光管的垂直朝向角；  
  
.  
  
.

### **2.外参标定**

**传感器之间的相对位置和朝向**，用3自由度的旋转矩阵和3自由度的平移向量表示  
  
.  
  
.

### **3.时间标定**

在原有离散时间融合模式下，简单地解决时间同步问题  
  
**离散时间**  
  
Online Temporal Calibration for Monocular Visual-Inertial Systems  
  
Online Temporal Calibration for Camera-IMU Systems: Theory and Algorithms  
  
**连续时间**  
  
3D Lidar-IMU Calibration based on Upsampled Preintegrated Measurements for Motion Distortion Correction  
  
a. kalibr 系列  
  
论文：Continuous-Time Batch Estimation using Temporal Basis Functions  
  
论文： Unified Temporal and Spatial Calibration for Multi-Sensor Systems  
  
论文： Extending kalibr Calibrating the Extrinsics of Multiple IMUs and of Individual Axes  
  
代码：https://github.com/ethz-asl/kalibr  
  
论文： Targetless Calibration of LiDAR-IMU System Based on Continuous-time Batch Estimation  
  
代码：https://github.com/APRIL-ZJU/lidar_IMU_calib  
  
.  
  
.

## **五、传感器融合**

### **卡尔曼融合案例**

通过扩展卡尔曼滤波器将立体摄像头传感器、惯性测量单元、【防盗标记–盒子君hzj】腿部测距和可选的间歇全球定位系统（GPS）位置更新融合在一起，以确保稳定、低延迟的性能

**文章知识点与官方知识档案匹配，可进一步学习相关知识**

[算法技能树](https://edu.csdn.net/skill/algorithm/?utm_source=csdn_ai_skill_tree_blog)[_首页_](https://edu.csdn.net/skill/algorithm/?utm_source=csdn_ai_skill_tree_blog)[_概览_](https://edu.csdn.net/skill/algorithm/?utm_source=csdn_ai_skill_tree_blog)_56530_ _人正在系统学习中_

 _显示推荐内容_
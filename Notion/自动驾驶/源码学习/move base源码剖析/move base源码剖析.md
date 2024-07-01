---

源码地址：[https://github.com/ros-planning/navigation](https://github.com/ros-planning/navigation)

![[post-images/Untitled 16.png|Untitled 16.png]]

## **move_base_node.cpp**

先来看`move_base_node.cpp`的源码，我们实际需要的程序是这个：

|   |   |
|---|---|
|`1``2``3``4``5``6``7``8`|`ros::init(argc, argv, "move_base_node");``ros::console::set_logger_level(ROSCONSOLE_DEFAULT_NAME,ros::console::levels::Debug);``tf::TransformListener tf(ros::Duration(10));``move_base::MoveBase move_base( tf );``ros::spin();``return(0);`|

## **MoveBase类**

### **MoveBase构造函数**

`MoveBase`构造函数开始对一些成员进行了列表初始化,没有值得说明的

### **第一部分**

|   |   |
|---|---|
|`1``2``3``4``5``6``7``8``9``10``11``12``13``14``15``16``17``18``19``20``21``22``23``24``25``26``27`|`// 枚举变量RecoveryTrigger，可取值有PLANNING_R, CONTROLLING_R, OSCILLATION_R``recovery_trigger_ = PLANNING_R;` `// as_维护movebase的actionServer状态机，并且新建了一个executeCb线程``as_ = new MoveBaseActionServer(ros::NodeHandle(), "move_base", boost::bind(&MoveBase::executeCb, this, _1), false);``ros::NodeHandle private_nh("~");``ros::NodeHandle nh;``// 给一系列参数设置默认值``std::string global_planner, local_planner;``private_nh.param("base_global_planner", global_planner, std::string("navfn/NavfnROS"));``private_nh.param("base_local_planner", local_planner, std::string("base_local_planner/TrajectoryPlannerROS"));``private_nh.param("global_costmap/robot_base_frame", robot_base_frame_, std::string("base_link"));``private_nh.param("global_costmap/global_frame", global_frame_, std::string("/map"));``private_nh.param("planner_frequency", planner_frequency_, 0.0);``private_nh.param("controller_frequency", controller_frequency_, 20.0);``private_nh.param("planner_patience", planner_patience_, 5.0);``private_nh.param("controller_patience", controller_patience_, 15.0);``private_nh.param("max_planning_retries", max_planning_retries_, -1);``private_nh.param("oscillation_timeout", oscillation_timeout_, 0.0);``private_nh.param("oscillation_distance", oscillation_distance_, 0.5);``// set up plan triple buffer, 声明3个vector指针``// 三个plan都是global_plan，最终由controller_plan_ 传给local planner: if(!tc_->setPlan(*controller_plan_))``planner_plan_ = new std::vector<geometry_msgs::PoseStamped>();``latest_plan_ = new std::vector<geometry_msgs::PoseStamped>();``controller_plan_ = new std::vector<geometry_msgs::PoseStamped>();`|

### **第二部分**

|   |   |
|---|---|
|`1``2``3``4``5``6``7``8``9``10``11``12``13`|`//建立planner线程``planner_thread_ = new boost::thread(boost::bind(&MoveBase::planThread, this));``//用于控制机器人底座``vel_pub_ = nh.advertise<geometry_msgs::Twist>("cmd_vel", 1);``current_goal_pub_ = private_nh.advertise<geometry_msgs::PoseStamped>("current_goal", 0 );``ros::NodeHandle action_nh("move_base");``action_goal_pub_ = action_nh.advertise<move_base_msgs::MoveBaseActionGoal>("goal", 1);``// we'll provide a mechanism for some people to send goals as //PoseStamped messages over a topic, they won't get any useful //information back about its status, but this is useful for tools like rviz``ros::NodeHandle simple_nh("move_base_simple");``goal_sub_ = simple_nh.subscribe<geometry_msgs::PoseStamped>("goal", 1, boost::bind(&MoveBase::goalCB, this, _1));`|

先是运行了一个线程`planner_thread_`，详细看`MoveBase::planThread()`。接下来一共涉及了4个`ros::NodeHandle`对象，注册了3个话题，订阅了话题`goal`，订阅话题的回调函数是`MoveBase::goalCB`

### **第三部分**

|   |   |
|---|---|
|`1``2``3``4``5``6``7``8``9``10``11``12``13``14``15``16``17``18``19``20``21``22``23``24``25``26``27``28``29``30``31``32``33``34``35``36``37``38``39``40``41``42``43``44``45``46``47``48``49``50``51``52``53`|`// /机器人几何尺寸有关参数 we'll assume the radius of the robot to be consistent with what's specified for the costmaps``private_nh.param("local_costmap/inscribed_radius", inscribed_radius_, 0.325);``private_nh.param("local_costmap/circumscribed_radius", circumscribed_radius_, 0.46);``private_nh.param("clearing_radius", clearing_radius_, circumscribed_radius_);``private_nh.param("conservative_reset_dist", conservative_reset_dist_, 3.0);``private_nh.param("shutdown_costmaps", shutdown_costmaps_, false);``private_nh.param("clearing_rotation_allowed", clearing_rotation_allowed_, true);``private_nh.param("recovery_behavior_enabled", recovery_behavior_enabled_, true);``//创建planner's costmap的ROS封装类， 初始化一个指针，用于underlying map``planner_costmap_ros_ = new costmap_2d::Costmap2DROS("global_costmap", tf_);``planner_costmap_ros_->pause();``//初始化global planner``try {` `// boost::shared_ptr<nav_core::BaseGlobalPlanner> planner_;` `// 模板类ClassLoader，实例化 BaseGlobalPlanner` `planner_ = bgp_loader_.createInstance(global_planner);` `planner_->initialize(bgp_loader_.getName(global_planner), planner_costmap_ros_);` `log.info("Created global_planner %s", global_planner.c_str());``} catch (const pluginlib::PluginlibException& ex) {` `log.fatal("Failed to create the %s planner, are you sure it is properly registered and that the containing library is built? Exception: %s", global_planner.c_str(), ex.what());` `exit(1);``}``//创建controller's costmap的ROS封装类， 初始化一个指针，用于underlying map``controller_costmap_ros_ = new costmap_2d::Costmap2DROS("local_costmap", tf_);``controller_costmap_ros_->pause();``//初始化local planner``try {` `tc_ = blp_loader_.createInstance(local_planner);` `log.info("Created local_planner %s", local_planner.c_str());` `tc_->initialize(blp_loader_.getName(local_planner), &tf_, controller_costmap_ros_);``} catch (const pluginlib::PluginlibException& ex) {` `log.fatal("Failed to create the %s planner, are you sure it is properly registered and that the containing library is built? Exception: %s", local_planner.c_str(), ex.what());` `exit(1);``}``// 开启costmap基于传感器数据的更新``planner_costmap_ros_->start();``controller_costmap_ros_->start();``//advertise a service for getting a plan``make_plan_srv_ = private_nh.advertiseService("make_plan", &MoveBase::planService, this);``//advertise a service for clearing the costmaps``clear_costmaps_srv_ = private_nh.advertiseService("clear_costmaps", &MoveBase::clearCostmapsService, this);``// if we shutdown our costmaps when we're deactivated... we'll do that now``if(shutdown_costmaps_){` `log.debug("[move_base] %s","Stopping costmaps initially");` `planner_costmap_ros_->stop();` `controller_costmap_ros_->stop();``}`|

`planner_costmap_ros_` 用于全局导航的地图

`controller_costmap_ros_` 用于局部导航的地图

GlobalPlanner类，继承`nav_core::BaseGlobalPlanner`, Provides a ROS wrapper for the global_planner planner which runs a fast, interpolated navigation function on a costmap.

### **第四部分**

|   |   |
|---|---|
|`1``2``3``4``5``6``7``8``9``10``11``12``13``14``15``16`|`// 载入recovery behavior, 这部分是状态机的设计问题，优先加载用户的设置,如果没有则加载默认设置``if(!loadRecoveryBehaviors(private_nh)){` `loadDefaultRecoveryBehaviors();``}``//initially, we'll need to make a plan``state_ = PLANNING;``//we'll start executing recovery behaviors at the beginning of our list``recovery_index_ = 0;``// start the action server``as_->start();``dsrv_ = new dynamic_reconfigure::Server<move_base::MoveBaseConfig>(ros::NodeHandle("~"));``dynamic_reconfigure::Server<move_base::MoveBaseConfig>::CallbackType cb = boost::bind(&MoveBase::reconfigureCB, this, _1, _2);``dsrv_->setCallback(cb);`|

### **MoveBase::executeCb**

每次有新的goal到来都会执行此回调函数, 如果有新目标到来而被抢占则唤醒planThread线程, 否则为取消目标并挂起处理线程

### **MoveBase::executeCycle**

`local planner`的核心工作函数, 只在`MoveBase::executeCb`中调用一次,它是局部路径规划的工作函数，其中会调用computeVelocityCommands来规划出当前机器人的速度
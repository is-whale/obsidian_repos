## **Velocity_set节点：**

该节点的主要工能是：接收safety_waypoints话题(astar_avoid输出路径)，结合实时点云信息，判断该路径上是否存在障碍物，如果存在障碍物，随机判断障碍物距离当前位置的距离，进而判断需要减速还是停车，进而修改当前位置到障碍物之间的路径点中的速度信息，避免突然减速；修改完成后发布修改后的路径finaobstacle_waypoint_pub_waypoints，以及障碍物所在的路径索引值obstacle_waypoint。

该节点会与Astar_avoid节点相配合，如果astar_avoid节点成功规划路径，那么就按照新规划的路径行驶，该节点不会有什么作用，如果astar_avoid节点路径规划失败或者没有可通过的路径，velocity_set节点会根据障碍物信息控制车辆停止或者减速。

**具体源码分析如下：**

- **主要话题**

订阅：

`safety_waypoints //astar_avoid节点发布的最终路径 points_no_ground //地面去除后的点云图`

发布：

`finaobstacle_waypoint_pub_waypoints //根据障碍物进行速度修改后的safety_waypoints obstacle_waypoint //障碍物的路径索引值(位置)主函数结构`

- **主函数结构**

`int main() { .... while(ros::ok()) { if (crosswalk.loaded_all && !crosswalk.set_points) crosswalk.setCrossWalkPoints(); ... EControl detection_result = obstacleDetection(closest_waypoint, vs_path.getPrevWaypoints(), crosswalk, vs_info, detection_range_pub, obstacle_pub, &obstacle_waypoint); changeWaypoints(vs_info, detection_result, closest_waypoint, obstacle_waypoint, final_waypoints_pub, &vs_path); .... } } 大体上说是： a) Crosswalk：斑马线检测(这里不作分析，暂时用不到) b) obstacleDetection()：检测障碍物位于当前线路的索引值， 并根据距离信息判断当前需要停车还是减速(标志位EControl::STOP、DECELERATE)。 c) changeWaypoints()：根据车辆需要的状态，修改后方waypoints中的速度信息， 进而在pure_presuit节点形成减速或者停车的效果。 d) 发布路径与障碍物位置`

obstacleDetection中包含pointsDetection()这和函数，而pointsDetection()又包含了detectStopObstacle() 和 detectDecelerateObstacle()两个重要的函数。其解析如下所示。

![[images/Untitled 31.png|Untitled 31.png]]

首先是decelerate obstacle，从车辆位置开始，向后遍历一定数量的路径点(蓝色点)，对每个路径点，遍历所有的点云，如果该路径点decelerate_range半径内不存在点云，则切换到下一个路径点，再次遍历所有点云；

直到路径点A，其decelerate_range半径内存在点云，且这些点云的数量在合理的范围内，则认为路径点A位置为decelerate obstacle。
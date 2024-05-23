<!--  
Simulate a carlike robot with the teb_local_planner in stage:  

- stage
- map_server
- move_base
- static map
- amcl
- rviz view  
    -->  
    <launch>  
    
    ```Plain
    <!--  ************** Global Parameters ***************  -->
    <param name="/use_sim_time" value="false"/>
    <!--*********************GPS****************************-->
    <!-- <include file="$(find chccgi610)/launch/realtime.launch"/> -->
    <!--*********************SCAN***************************-->
    <include file="$(find rslidar_sdk)/launch/start.launch"/>
    <node pkg="pointcloud_to_laserscan" type="pointcloud_to_laserscan_node" name="pointcloud_to_laserscan"/>
    <include file="$(find transformer_tf)/launch/transformer_tf.launch"/>
    <!--**********************BASE**************************-->
    <include file="$(find hunter_bringup)/launch/hunter_robot_base.launch"/>
    <include file="$(find localization)/launch/run_localization_gaoxin.launch"/>
    <!--  ************** Navigation ***************  -->
    <node pkg="move_base" type="move_base" respawn="false" name="move_base" output="log">
    <rosparam file="$(find teb_local_planner_tutorials)/cfg/carlike/costmap_common_params.yaml" command="load" ns="global_costmap" />
    <rosparam file="$(find teb_local_planner_tutorials)/cfg/carlike/costmap_common_params.yaml" command="load" ns="local_costmap" />
    <rosparam file="$(find teb_local_planner_tutorials)/cfg/carlike/local_costmap_params.yaml" command="load" />
    <rosparam file="$(find teb_local_planner_tutorials)/cfg/carlike/global_costmap_params.yaml" command="load" />
    <rosparam file="$(find teb_local_planner_tutorials)/cfg/carlike/teb_local_planner_params.yaml" command="load" />
         <param name="base_global_planner" value="global_planner/GlobalPlanner" />
            <param name="planner_frequency" value="1.0" />
            <param name="planner_patience" value="5.0" />
    
            <param name="base_local_planner" value="teb_local_planner/TebLocalPlannerROS" />
            <param name="controller_frequency" value="5.0" />
            <param name="controller_patience" value="15.0" />
    
            <param name="clearing_rotation_allowed" value="false" /> <!-- Our carlike robot is not able to rotate in place -->
            <remap from="cmd_vel" to="twist_command"/>
    </node>
    
    <!--  ****** Maps *****  -->
    <!-- ************transform /odom to /map ****************** -
    
    <!--  **************** Visualisation ****************  -->
    <node name="rviz" pkg="rviz" type="rviz" args="-d $(find teb_local_planner_tutorials)/cfg/rviz_navigation_pp.rviz" output="log"/>
    <!-- ************transform /odom to /map ****************** -->
    <node name="odom_map_transfer" pkg="odomtransform" type="ot" output="log" />
    <node name="path_buffer" pkg="PathBuf" type="pathbuf" output="screen" />
    <node name="pure_pursuit" pkg="pp" type="ppp" output="log" >
    <!-- <remap from="smart/cmd_vel2" to="twist_command"/> -->
    </node>
    </launch>
    
    
    ```
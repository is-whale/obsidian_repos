1. 选定功能包右击 ---> 添加 launch 文件夹
2. 选定 launch 文件夹右击 ---> 添加 launch 文件
3. 编辑 launch 文件内容

```JavaScript
<launch>
<node pkg="helloworld" type="demo_hello" name="hello" output="screen" />
<node pkg="turtlesim" type="turtlesim_node" name="t1"/>
<node pkg="turtlesim" type="turtle_teleop_key" name="key1" />
</launch>
```

1. node ---> 包含的某个节点pkg -----> 功能包type ----> 被运行的节点文件name --> 为节点命名output-> 设置日志的输出目标

==**output="screen 表示将此节点的输出发送到终端**==

1. 运行 launch 文件
    
    `roslaunch 包名 launch文件名`
    
2. 运行结果: 一次性启动了多个节点
map->odom之间没问题

odom->base_footprint之间交给transformer_tf那个包，订阅odom话题，发布TF关系

然后hunter2.0驱动里面会发布odom->base_link间的关系，把launch文件里面关于TF发布的参数赋予成false；

base_footprint->base_link之间发布静态关系；

base_link和其他传感器的关系保持原装
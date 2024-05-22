docker run [OPTIONS] IMAGE [COMMAND]  
OPTIONS  
--name="新容器名字"：为容器指定一个新名称  
-d：后台运行容器，并返回容器ID，也就是启动守护式容器  
-i：以交互模式运行容器，通常与-t配合使用  
-t：为容器重新分配一个伪输入终端，通常与-i配合使用  
-P：随机端口映射  

docker run -it IMAGE ID

  

docker ps 查看当前正在运行的所有容器（ps为linux命令，查看当前进程）  
OPTIONS  
-a： 列出当前所有正在运行的容器 + 历史上运行过的  
-l：显示最近创建的容器  
-n num ：显示最近创建的n个容器  
-q：静默模式，只显示容器的编号  

启动容器  
docker start CONTAINER_ID  
docker restart CONTAINER_ID
子仓库

git submodule update --init --recursive

### 1.基础概念

分布式仓库

.git 文件（核心）

|—config

### 2.工作流

![[images/Untitled 9.png|Untitled 9.png]]

Workspace： 工作区，就是你平时存放项目代码的地方

Index / Stage： 暂存区，用于临时存放你的改动，事实上它只是一个文件，保存即将提交到文件列表信息

Repository： 仓库区，就是安全存放数据的位置，这里面有你提交到所有版本的数据。其中HEAD指向最新放入仓库的版本

Remote： 远程仓库，托管代码的服务器

![[images/Untitled 1 3.png|Untitled 1 3.png]]

### 3.基础命令

git add.

  

### 4.不同场景的使用以及技巧

1.使用ssh密钥加速访问（推荐，因为速度较快，并且不需要输入账号密码）

`ssh-keygen -t rsa -C "注册的邮箱"`

2.同步已有代码文件夹

3.同步已有本地仓库

4.同步其他远端仓库

### 忽略中间文件

.gitingore

```JavaScript
build/
*.o
*.log
test.c
```

### 5.使用编辑器插件简化操作

如vscode
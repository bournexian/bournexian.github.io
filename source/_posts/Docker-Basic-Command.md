---
title: Docker_Basic_Command_01
date: 2017-12-10 21:03:41
tags: docker
---

### DOCKER BASIC COMMAND -- container & images

#### Prerequisites
* Docker for Windows installed 
* Basic knowledge of linux/powershell command
* DockerID registered

如果选择的是windows平台, 可以安装docker for windows. 

NOTE: 由于默认源是从docker hub官方地址拉取镜像,建议注册阿里云账号并更换源地址为阿里云加速地址.
在Settings->Daemon中配置Registry mirrors:![Change_registry_mirrors_address](Change_registry_mirrors_address.png)
更换源后貌似还需要删掉默认源地址，具体的配置文件路径忘了...

如果选择的是linux平台，可以选择安装linux系统； 对于linux不熟悉的可以使用Online environment [docker classroom](http://training.play-with-docker.com).

#### Run the Docker containers
Docker 容器能够以三种不同方式来运行:
1. 执行单独的任务. e.g. Shell script 或者指定的程序. 
2. 交互式地运行. 连接到容器内的虚拟环境, 类似使用SSH连接到远程服务器.
3. 在后台运行.  通常在一些长时间运行的服务里用到，比如Web服务器或者数据库.

基本命令
> docker container [commands]

下面介绍的是Windows下面的命令，确保设定使用的是Windows containers而非Linux containers.

* 单独任务
> docker container run microsoft/nanoserver powershell hostname

    比如上面的命令是使用nanoserver镜像创建容器，然后再在容器里面运行hostname命令.

    由于nanoserver镜像在本地不存在，所以就会从docker hub (官方仓库) /配置的源拉取镜像.
    在执行完命令hostname后，就会退出container.

* 交互式
> docker container run -it --rm microsoft/nanoserver powershell

    比如上面命令是在容器里打开powershell命令，之后交互地执行命令; 

    -it  参数分别代表interactive和tty，意味着交互式地运行容器，并且连接一个命令行的session.
    --rm 参数代表当容器推出后自动删除容器. 

* 后台运行
> docker container run -d -p 80:80 --name iis microsoft/iis

    上面命令是后台创建一个容器运行IIS服务.

    -d 参数代表detach，意味着后台运行.
    -p 参数发布端口，将容器内的iis的80端口mapping到host机器的80端口.


如何查找后台运行的容器呢？可以通过以下命令:

> docker container ls

or

> docker ps

-a 参数可以列出运行过但已经退出的容器.

#### The images
列出本地已经拉取的镜像列表.
>docker images

从镜像仓库拉取镜像或者发布镜像
>docker pull/push

从tar文件导入image
>docker import

NOTE：
命令太多就不详述，使用--help查阅帮助文档可以获取详细的使用方法:
> [commands] --help

对于每个参数都有详细解释. 类似linux里面的man.


另外除了命令行，还可以使用图形界面管理docker: **Kitematic**
![Kitematic](Kitematic.png)

可以搜索镜像，查看自己publish到仓库的镜像，查看本地镜像，创建container等，很方便。

Reference:
[docker lab](https://github.com/docker/labs)
[play with docker](http://training.play-with-docker.com)
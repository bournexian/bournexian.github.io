---
title: Docker-Swarm
date: 2017-12-28 22:37:31
tags: docker
---

### Docker -- Swarm

#### Introduction to Docker Machine

##### Docker Engine
在使用Swarm之前，个人认为有必要先介绍下Docker Machine的概念。
我们口中所说的docker，指的是docker engine， 包含三个部分：
* docker daemon
* REST API
* docker CLI

![engine.png](engine.png)

安装docker engine的主机就是host，我们通常使用docker CLI的命令透过REST API和本机的docker daemon打交道。

##### Docker Machine

与在host机器安装docker engine相比，docker machine则是通过REST API与远程机器的docker daemon通讯，使用的是docker machine CLI。 通过docker machine create命令可以在本地网络创建docker host (e.g.在本机的VirtualBox新建一个linux虚拟机，然后在此虚拟机里安装docker engine)， 也可以在云服务如Azure，AWS上面创建docker host。
docker machine create通常会生成证书用于认证，以及配置远程机器监听的TCP端口，然后就可以在本机管理远程的机器上面运行的docker daemon。如下图所示：

![machine.png](machine.png)

#### Swarm

Swarm可以理解成一个集群， 使用docker swarm init可以启用swarm模式。 而集群里面分为manager和worker两种角色，每台实体机或者虚拟机可以视为一个node。

下面以Windows下为例子，使用hyperv作为docker machine的driver创建两个docker host:
首先到Virtual Switch Manager创建一个virtual switch，命名为myswitch， 然后执行命令：

    docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm1

这个命令会使用boot2docker.iso镜像来生成一个VM，并在VM里面安装docker engine，配置相关连接的证书和监听端口。 其中输出如下图:

![docker_machine_create.png](docker_machine_create.png)

继续使用相同命令创建另外一个VM并命名为myvm2:

    docker-machine create -d hyperv --hyperv-virtual-switch "myswitch" myvm2

查看刚才创建的machines：

    docker-machine ls

使用ssh协议连接到myvm1，在该VM上初始化SWARM，默认为manager角色:

    docker-machine ssh myvm1 "docker swarm init"

使用ssh协议连接到myvm2，在该VM上执行join命令，作为worker角色添加到swarm:

    docker-machine ssh myvm2 "docker swarm join --token <token> <ip>:2377"

添加完后可以使用docker node 在manager角色的机器上面打印出现有的节点状态：

    docker-machine ssh myvm1 "docker node ls"

![docker_node_ls.png](docker_node_ls.png)

如上图显示，有3个节点，第一个节点是在host机器安装的docker engine，因为未加入swarm集群，所以status是Down，但是因为docker daemon仍然在运行，所以availability是Active。 而Manager节点ID后面会有个星号，并Manager Status显示为Leader，worker节点则没有。  

顾名思义，Manager Node主要负责管理调度的工作，Worker主要职责是执行Manager分配的任务，但需要注意的是，Manager Node不单单负责管理职能，它同时作为一个Worker，会有容器运行在其上面。

另外，除了上面使用的docker-machine ssh "<command to be invoked in the VM>"这种格式外，还能使用交互shell方式连接，这样session会保留直到关闭命令行窗口，如要连接myvm1则执行:

    docker-machine env myvm1

然后按照提示执行：

    docker-machine env myvm1 | Invoke-Expression

这样再执行docker命令就是在直接对myvm1的docker engine上面执行命令。
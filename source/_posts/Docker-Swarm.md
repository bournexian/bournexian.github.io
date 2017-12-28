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

TBD...
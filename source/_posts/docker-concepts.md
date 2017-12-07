---
title: docker_concepts
date: 2017-12-07 10:43:22
tags: docker
---

### DOCKER CONCEPTS

随着近年来DevOps以及微服务概念的流行， Docker技术变得火热. 
最近有时间可以学习下，顺带记录下笔记. 

Docker 基础概念

#### Image:
An image is a lightweight, stand-alone, executable package that includes everything needed to run a piece of software, including the code, a runtime, libraries, environment variables, and config files.

换做以前的概念，可以理解成一个打包好的“ISO”，不但操作系统，代码，运行时等都囊括了； 
使用了Union FS系统，可以理解成有很多层，如BaseImage 是操作系统层 e.g.Ubuntu; 其上层可以是Nginx image/Apache image; 依赖于增量式保存本层和上一层的差异，所以比较轻量size也比叫小.

Pros: 
由于所有依赖(甚至操作系统)都包含在镜像中， 所以可以确保无论部署在哪个环境中都有相同的效果。 跟以前只打包程序，然后发布到实体机/虚拟机服务器相比，不会出现一些环境配置差异的问题； 比如最近就遇到QA环境打了SQLServer 补丁而Dev machine没有打补丁，发现behavior不一致的问题，不利于调试； 

![DockerImage_layers](docker_image_layers.jpg)

#### Container:
A container is a runtime instance of an image—what the image becomes in memory when actually executed. It runs completely isolated from the host environment by default, only accessing host files and ports if configured to do so.

假如把Image类比做“ISO”， 那么容器可以理解成轻量级的“虚拟机”，上面说的Image是不能做修改的，只能把Image加载到容器中，我们才能基于它增加一层可写层，但这层可写层是只能容器内可见并且重启后会丢失的；
做类比的话，可以理解是"ISO"加载到了"虚拟机"进行了光驱启动，但是下次再开机就会丢失上次的改动了；假如要保留改动，就要保存到外部的"Volumes".

![container-layers](container-layers.jpg)

#### Volumes：
Volumes are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.

由于上面说的原因， container的生命周期结束后，容器的可写层就会跟随销毁。故通常Docker中常用的持久化数据的方式是使用Volumes. Volume的优点有：
* Volumes can be more safely shared among multiple containers.
* Volumes are easier to back up or migrate than bind mounts.
* Volume drivers allow you to store volumes on remote hosts or cloud providers, to encrypt the contents of volumes, or to add other functionality.

其实除了volume外，还有另外两种方法:
* bind mount
当时用绑定挂载的时候，host主机的一个文件或者目录会挂载到容器里面. 感觉像linux 里面的link链接.
Question:
同一目录当能否绑定挂载到多个container里面？？ 有待试验

* tmpfs mount
用于临时存放一次性数据，存储在host机器的内存(或者swap). 而且当容器停止后， tempfs就会自动remove; 容器保存的时候也会exclude tempfs.

![types-of-mounts-volume](types-of-mounts-volume.png)



#### 总结
描述了一下Docker里面几个基本的概念，具体使用的命令将留到下篇继续.
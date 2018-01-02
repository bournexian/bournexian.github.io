---
title: Docker Services
date: 2017-12-23 22:37:03
tags: docker
---

### Docker -- Services

#### About services

通常一个应用都由不同部分组成，比如一个网站会有Web服务器提供Web服务，数据库提供存储服务，还有其它一些组件组成。 Service可以看作定义了单个组件（或者说服务）是如何运行image的，e.g.定义 开放了哪些端口，有多少个副本（或者说实例）等。 而这些配置的定义，是通过docker-compose.yml文件实现的。

仔细想来，之前提过的Dockerfile不就是定义了如何运行image的，这里回顾一下Dockerfile的作用是： 方便复用。 只需要记录下baseimage和配置的过程，把这个文件share出去，别人运行的时候也能复现该结果。减免把image以及修改进行打包分享的麻烦（因为这样size会比较大）。

那么Dockerfile与docker-compose有什么联系和区别？ 它们很相似，只不过Dockerfile是记录单个服务的构建过程，而docker-compose则记录一组服务的构建，以及如何把这些服务连接起来的信息。


#### Docker-compose.yml

docker-compose文件的官网例子：

    version: "3"
    services:
      web:
        # replace username/repo:tag with your name and image details
        image: bourne/get-started:part2
        deploy:
          replicas: 5
          resources:
              limits:
                cpus: "0.1"
                memory: 50M
          restart_policy:
              condition: on-failure
        ports:
            - "80:80"
        networks:
            - webnet
    networks:
      webnet:

其中services就是定义了各服务，其中可以看见有一个web结点定义了web服务的构建以及一个networks节点定义了network服务。  Web service节点下面又指定了：
* 使用的image
* deploy的实例数目是5即启动5个container，并且每个container限制使用10%的CPU和50M的内存
* 有container fails就执行重启
* 配置mapping的端口
* 配置使用的网络


#### Run the docker-compose.yml to build the app

要执行docker-compose文件之前，需要开启swarm， 否则会报错。 执行以下命令初始化swarm:

    docker swarm init

> 假如该主机有多个IP，则需要用 --advertise-addr [IP addr] 来指定具体的IP初始化swarm。

接着就可以使用stack命令来运行创建的docker-compose文件：

    docker stack deploy -c docker-compose.yml getstartedlab

接着就可以通过命令查看服务：
* 查看服务
    docker service ls
* 查看某个服务的状态
    docker service ps getstartedlab_web
* 更新docker-compose.yml文件后，再次执行docker stack deploy部署命令则会自动update （in-place update）

![services.png](services.png)

通过curl访问可以看到每次都print出来不同的container id，可见实现了负载均衡。

![curl_lb.png](curl_lb.png)


#### Take down the services/app

使用docker stack rm 来铲掉之前创建的services：
    
    docker stack rm getstartedlab

退出swarm模式：

    docker swarm leave --force


#### Conclusion

这篇主要围绕docker-compose.yml展开，也涉及了swarm初步的使用，下节将会试下使用swarm创建集群。





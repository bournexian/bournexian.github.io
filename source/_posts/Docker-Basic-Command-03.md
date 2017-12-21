---
title: Docker-Basic-Command-03
date: 2017-12-18 22:46:32
tags: docker
---

### DOCKER BASIC COMMAND 03

接下来准备按tutorial实践下存储方面的命令, 本篇主要是关于volume的。

#### Volume
    
    docker volume [Commands]
    Commands:
    create      Create a volume
    inspect     Display detailed information on one or more volumes
    ls          List volumes
    prune       Remove all unused volumes
    rm          Remove one or more volumes

只有这几个命令，很容易使用；举例创建一个名字为myvol_new的volume，然后查询的命令分别为：
    docker volume create myvol_new
    docker volume inspect myvol_new

其输出为:

    [
        {
            "CreatedAt": "2017-12-18T14:38:42Z",
            "Driver": "local",
            "Labels": null,
            "Mountpoint": "/var/lib/docker/volumes/myvol_new/_data",
            "Name": "myvol_new",
            "Options": {},
            "Scope": "local"
        }
    ]


#### Create Volume Automatically & Mount Option

除了直接创建volume外，也可以通过启动container或创建service的时候挂载容器，假如容器此前不存在则会自动创建。
如下面的例子就是以busybox镜像创建名为test的container，挂载volume到容器内的/app目录，如myvol此前不存在则会自动创建此volume。

    docker run -d \
    -it \
    --name test \
    --mount source=myvol,target=/app \
    busybox:latest


创建服务类似，服务可以指定创建多少个实例，使用--replicas参数，比如下面这个例子是创建4个container， 而且挂载myvol2到容器内的/app目录。
值得注意的是，这个volume是多个container共享的。  加上readonly参数则是表示容器内对该挂载目录内容只读。

    docker service create -d \
    --replicas=4 \
    --name test-service \
    --mount source=myvol2,target=/app, readonly \
    busybox:latest


另外有一点值得一提，假如启动容器的时候创建了一个新容器，如上面的例子挂载到了/app目录，假如这个容器的该目录（挂载的目录）已经存在文件或者目录，那么内容将会被拷贝到volume。 而且由于被拷贝到volume，其它挂载了该容器的container也能够访问这些内容。 下面给出一个例子，假如挂载的目录是/usr，已经存在的/sbin则会copy到volume中:

![pre-populate](pre-populate.png)

#### Volume Driver

Docker可以使用不同的volume driver， 比如通过下面命令来通过安装插件的方式安装新的volume driver:
    
    docker plugin install --grant-all-permissions vieux/sshfs

然后创建volume的时候可以指定volume driver:
    
    $ docker volume create --driver vieux/sshfs \
    -o sshcmd=test@node2:/home/test \
    -o password=testpassword \
    sshvolume



#### Conclusion

这篇就暂时到这里，主要列出了一些volume的用法。 下篇打算动手了解下tempfs 和 bind mounts。
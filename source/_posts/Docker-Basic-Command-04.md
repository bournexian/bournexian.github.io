---
title: Docker-Basic-Command-04
date: 2017-12-22 15:48:13
tags: docker
---

### DOCKER BASIC COMMAND -- Bind Mount & Tmpfs

除了常用的volume外，还有bind mounts以及tmpfs两种方式存储数据； 在concepts那篇文章简单介绍过，bind mount类似于link，把host主机的目录挂载到container内， 而tmpfs则存储在内存中，用于存放临时数据。 下面直接试试用法。

#### Bind mount

在docker 17.06后，对于挂载volume或者bind mount，推荐使用--mount参数，而以前的-v参数暂时仍保留。
Bind mount不像volume可以单独创建，必须和容器关联使用，而且依赖于host机器的文件系统有特定的目录结构，和volume类似。
官网说几个地方跟我实际操作有一点差异，

>   When you use a bind mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its full or relative path on the host machine. By contrast, when you use a volume, a new directory is created within Docker’s storage directory on the host machine, and Docker manages that directory’s contents.

这里说文件或目录是通过host machine的full/relative path关联，但是注意的是实际使用命令的时候要指定full path, 否则报这个错: invalid mount path: './target' mount path must be absolute.


>   The file or directory does not need to exist on the Docker host already. It is created on demand if it does not yet exist. 

第二个，这里说文件或目录不需要已经存在，如果不存在的话会即时创建。 但实际上操作会遇到错误： bind source path does not exist.  不清楚我哪里的操作有问题，或者跟使用的docker版本有关。



下面例子是运行一个container，并为host主机当前目录下面的target目录创建bind mount，mapping到容器里的/app目录：

    $ docker run -d \
    -it \
    --name devtest \
    --mount type=bind,source="$(pwd)"/target,target=/app \
    busybox:latest

这里解释下$(pwd)的意思是执行pwd命令，就是获取当前working directory的路径。
使用docker inspect devtest，可以查看到Mounts section判别Type为“bind”即为bind mount。

    "Mounts": [
            {
                "Type": "bind",
                "Source": "/root/target",
                "Destination": "/app",
                "Mode": "",
                "RW": true,
                "Propagation": "rprivate"
            }
        ],


值得注意的是，假如container里挂载的目录已经有数据，那么将会把目标目录清空，举个极端的例子，假如把/root/target目录挂载到容器的/usr目录，/usr目录的内容会被清空：

![bind_mount_target_diralreadyexists](bind_mount_target_diralreadyexists.png)


另外可以配置bind mount的挂载选项，比如只读，加上flag readonly即可；
同时细心的会发现bind mount跟volume相比，少了“Driver”而多出一个"Propagation"。 Driver是volume独有的，可能是因为bind mount是依赖于宿主机的文件系统，无法定义storage driver。 而propagation这个是跟mount相关的，比如目标机器挂载了 /mnt, 同时这个目录也挂载到了 /tmp. Propagation 设置会决定是否会在挂载/tmp/a的同时允许/mnt/a同时也自动挂载上。 这个propagation算是比较进阶的技能，详细的可以查看文档。


#### Tmpfs

当要保存一些一次性数据的时候，可以使用tmpfs，但要注意的是tmpfs有一些局限性：

* tmpfs mounts cannot be shared among containers.
* tmpfs mounts only work on Linux containers, and not on Windows containers.

而具体例子如下，因为使用宿主机的内存(或swap)，运行container的时候只需要指定容器内的挂载目录即可：

    docker run -d \
    -it \
    --name tmptest \
    --mount type=tmpfs,destination=/app \
    busybox

还有一些options可以指定：

* tmpfs-size	
Size of the tmpfs mount in bytes. Unlimited by default.

* tmpfs-mode	
File mode of the tmpfs in octal. For instance, 700 or 0770. Defaults to 1777 or world-writable.



#### Conclusion

至此Docker storage方面的三个重要功能都过了一遍， 接下来会试试Docker services, swarm, stack等进阶命令。

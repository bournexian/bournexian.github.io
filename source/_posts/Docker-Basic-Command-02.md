---
title: Docker_Basic_Command_02
date: 2017-12-12 22:46:59
tags: docker
---

### DOCKER BASIC COMMAND 02

上篇文章讲了一下基础的命令，这篇将继续围绕镜像展开。
NOTE: Win 环境下要切换container模式为Linux

#### Dockerfile
使用docker run可以创建一个镜像的实例 即容器，举个例子我要使用python框架运行一个web应用，那么我就要使用python镜像运行一个容器，然后在此基础上我需要安装Flask框架以及Redis存储系统，最后再执行python程序setup起一个web服务.

那么有没办法基于python镜像的基础上，添加我们自己的应用，然后打包成一个新镜像呢？ -- Dockerfile就是应用在此的.

下面就使用一个docker官网的例子说明， 新建一个目录，然后cd到目录内，创建文件Dockerfile，内容如下:
    # Use an official Python runtime as a parent image
    FROM python:2.7-slim

    # Set the working directory to /app
    WORKDIR /app

    # Copy the current directory contents into the container at /app
    ADD . /app

    # Install any needed packages specified in requirements.txt
    RUN pip install --trusted-host pypi.python.org -r requirements.txt

    # Make port 80 available to the world outside this container
    EXPOSE 80

    # Define environment variable
    ENV NAME World

    # Run app.py when the container launches
    CMD ["python", "app.py"]

上面的命令其实就是指定python 作为baseimage, 在此基础上指定工作目录，拷贝当前目录进容器，根据requirements.txt列出的包名安装软件，暴露端口，执行python应用.

创建requirements.txt, Dockerfile中会通过python自带的pip包管理器安装这个文件里所列出的软件包名, 内容如下:

    Flask
    Redis

创建文件app.py, 这个是python程序文件，使用了Redis存储和Flask框架创建一个web服务并监听80端口请求，内容如下:
```python
    from flask import Flask
    from redis import Redis, RedisError
    import os
    import socket

    # Connect to Redis
    redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

    app = Flask(__name__)

    @app.route("/")
    def hello():
        try:
            visits = redis.incr("counter")
        except RedisError:
            visits = "<i>cannot connect to Redis, counter disabled</i>"

        html = "<h3>Hello {name}!</h3>" \
            "<b>Hostname:</b> {hostname}<br/>" \
            "<b>Visits:</b> {visits}"
        return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

    if __name__ == "__main__":
        app.run(host='0.0.0.0', port=80)
```
有了以上三个文件后，我们就可以创建一个新镜像了.

#### Build image

    docker build -t friendlyhello .

使用上面的命令创建新镜像，其中-t是--tag list 的缩写，可以指定镜像的名字以及tag； 在这个例子里面没有指定tag， 就使用默认值latest. 而末尾 . 的意思是build使用的Dockerfile的路径在当前目录.
> tag 就是镜像的标签， 比如当我们去pull image的时候，想pull的python版本是2.7而不是3.6的时候就可以指定tag. e.g. python:2.7.  

如无意外，一个新的镜像就创建好了； 使用上篇使用过的命令来执行:

    docker run -p 4000:80 friendlyhello

这样会把容器内部的80端口，mapping到host机器的4000端口，打开浏览器访问localhost:4000 就可以访问到一个测试的页面.
![friendlyhello running in Browser](friendlyhello.png)

由于默认会把log输出，press Ctrl+C 退出，但容器仍然在后台运行，可以使用docker ps -a 查看. 

假如此时想用interactive mode去连接进container，可以使用container名字也可以使用container id （可以取前4位简写）, 执行命令：

    docker exec -it 5ebc /bin/bash

假如想停掉这个容器的话，可以使用:

    docker container stop 5ebc

#### Share image
可以将上面步骤创建的image发布到docker hub，来共享自己创建的镜像。
使用命令登陆dockerid:

    docker login

使用命令为image打tag:

    docker tag [yourimagename] username/repository:tag

Upload镜像:

    docker push username/repository:tag


#### Conclusion
本篇主要介绍了下image相关的基础命令和如何上传到软件仓库. 

---
title: deeplearning_part1
date: 2017-07-29 21:25:35
tags: deeplearning
---

## 深度学习从入门到放弃 篇一 环境准备

最近刚开始看一个公开课 [CS231n Convolutional Neural Networks for Visual Recognition](https://cs231n.github.io/)

听起来名字很高大上... 不过很多概念不是很懂，看来要先学习一下基础知识，比如一些概念和公式。


就先硬着头皮跟着课上吧，课程是基于Python的，首先要准备环境。
打算在Windows系统下面跑，然后折腾了一天, 后来发现原来有安装包可以一次性安装大部分python科学计算方面的软件包，推荐使用 Anaconda.

如果想要一步步搭建环境，可以follow下面的来独立安装需要的库：
* Python 2.7/3.5
> 这里要注意的是安装 64bit 版本的，否则有内存使用限制2GB，后面跑代码载入图片数据报错
> 安装完之后就可以使用pip包管理工具来安装所需要的组件或者库， 类似于node的npm或者ubuntu的apt

    pip install [packagename]
* Numpy
> Python下面科学计算的库，通常用来操作向量和线性代数。建议用wheel方式安装，原因下面会解释。

* SciPy
> 应该类似Numpy，依赖于Numpy+mkl, MKL是Intel的Math Kernel Library，在windows下面要安装很麻烦，所以建议使用wheel方式安装而非pip下载源码编译安装。

* Mathplotlib
> 用来数据可视化的，比如画图画表

* Microsoft Visual C++ Compiler for Python 2.7
> 安装一些软件的时候会用到这个编译工具, 我用的2.7版安装对应2.7的，3.5的话不清楚是否有对应的？

* Pandas
> 这个好像课程中没用到，数据管理的。

* Jupyter Notebook
> 分段执行python代码，第一次用的时候感觉很新鲜啊~
![Jupyter](Jupyter.png)


环境配好之后，可以在Jupyter Notebook上面运行一些代码，比如下面的一些tutorial，如果没问题环境就搭建成功啦~

>python, numpy, Jupyter的一些quick tutorial:
[https://cs231n.github.io/python-numpy-tutorial/](https://cs231n.github.io/python-numpy-tutorial/)
[https://cs231n.github.io/ipython-tutorial/](https://cs231n.github.io/ipython-tutorial/)


---
title: '02-VTA使用 简明教程'
date: 2024-04-29
permalink: /posts/2024/04/blog-post/
date created: 星期二, 四月 30日 2024, 4:10:17 下午
date modified: 星期二, 四月 30日 2024, 5:26:53 下午
---
%%Target：将ResNet50在VTA上跑起来%%

**需要在安装好tvm之后进行**

> 参考文章：[VTA 安装指南 | Apache TVM 中文站 (hyper.ai)](https://tvm.hyper.ai/docs/topic/vta/install/)

## VTA安装

### 1. 配置

~/.bashrc 里设置路径

```bash
export TVM_PATH=<path to TVM root>
export VTA_HW_PATH=$TVM_PATH/3rdparty/vta-hw
%设置好之后记得执行一下source ~/.bashrc
```

启用 VTA 功能模拟库，进入到/tvm/build后执行如下代码：

```bash
echo 'set(USE_VTA_FSIM ON)' >> build/config.cmake
cmake .. 
make -j4
```

很熟悉对吧， 所以也可以直接进入到config.cmake文件里搜索VTA，把原来的OFF改成ON

make完成之后，把VTA添加到Python路径下

```bash
export PYTHONPATH=/path/to/vta/python:${PYTHONPATH}
```

还记得怎么添加路径吗？在~/.bashrc里
/path/to/vta的路径是哪个呢？ 请仔细观看/vta的文件夹，就会发现里面有一个python的文件夹，所以这个路径应该是/tvm/vta
	添加好之后请记得执行 source ~/.bashrc
	% 这里如果你使用了conda创建的虚拟环境，可能会在source之后回到base，这是conda本身导致的，解决方案是重新再activate一下自己的环境就好了

通过 `echo $PYTHONPATH`
可以看到当前配置的python路径

### 2. 测试

> 参考文章 [# TVM：深度学习框架编译器的安装踩坑集](https://blog.csdn.net/mmphhh/article/details/116484914)

为确保已正确安装 VTA Python 包，运行以下 2D 卷积进行测试。

```bash
python <tvm root>/vta/tests/python/integration/test_benchmark_topi_conv2d.py
```

能够正常运行说明测试通过

可能报错：

1. **报错1：**ModuleNotFoundError: No module named 'pytest'

原因：python3.7自身没有安装pytest模块
解决方法：conda install pytest

2. **报错2：**TVM出现download的错误，比如：WARNING:root:Failed to download tophub package for llvm: <urlopen error [Errno 111] Connection refused>问题。

 去GitHub上，下载[tophub](https://github.com/tlc-pack/tophub)的工程

```bash
git clone https://github.com/tlc-pack/tophub.git
```

并把这个工程放在下面的Linux路径下。(注意tophub文件夹不能嵌套，即用下载下来的tophub替换原来的tophub，替换前可以看一眼，原来的tophub是空的)

```bash
~/.vta/tophub
```

---
title: docker小实践
date: 2020-02-29 12:56:34
tags:
  - 技术
---
### 为啥想整docker了

记得大一上学期，参加工作室招新的时候，一共没做几道题，其中一个就是linux

折腾好半天装好虚拟机，搞了远程桌面啥的，这题目的分就算到手了

但是题目的进阶知识推荐里，有docker的简单介绍，当时忙着做其他题目，就将这个搁下了

最近重学前端，按理来说跟docker没什么相关的东西，但是py和java的一点小基础让自己萌生了“全干工程师”的想法（误）

正好linux也倒腾了很久，一年里换了ubuntu、centos、manjaro、kali、wsl，也算对操作系统有了比较粗浅的认知，于是对docker技术也充满好奇

好奇才是推动自己学习的动力呀！

打开天池竞赛，突然发现了docker的入门指南和小比赛

8说了，直接开冲！

### windows with docker

因为自己的机器还是dell xps-9360，装的是原装win，所以会有些小问题（我会有mac的！）

#### 第一步，看一下自己的win版本

打开控制面板->系统与安全->系统

发现自己的版本是windows10 家庭中文版

但是docker是要求win10 专业版的，所以要升级

行8，升级部分略过，至于怎么做嘛，建议tb

#### 第二步，开启虚拟化

打开任务管理器

[![3ywXz6.png](https://s2.ax1x.com/2020/02/29/3ywXz6.png)](https://s2.ax1x.com/2020/02/29/3ywXz6.png)

#### 第三步，启动hyper-V

控制面板->程序

[![3yBQBD.th.png](https://s2.ax1x.com/2020/02/29/3yBQBD.th.png)](https://s2.ax1x.com/2020/02/29/3yBQBD.th.png)

点击这几个选项，然后重启

#### 第四步，安装docker

[官网链接](https://www.docker.com/get-started)

安装之后，还要注册账号哦，windows里有desktop，操作起来还算简单

小鲸鱼！出现吧！

[![3yDixP.th.png](https://s2.ax1x.com/2020/02/29/3yDixP.th.png)](https://s2.ax1x.com/2020/02/29/3yDixP.th.png)

#### 第五步，编写在此次任务中需要的任务文件

新建一个文件夹，出于习惯叫tianchi_submit_demo

[![3yD3rT.th.png](https://s2.ax1x.com/2020/02/29/3yD3rT.th.png)](https://s2.ax1x.com/2020/02/29/3yD3rT.th.png)

里面要有四个文件

1. Dockerfile

里面的内容是

```
# Base Images
## 从天池基础镜像构建
FROM registry.cn-shanghai.aliyuncs.com/tcc-public/python:3

## 把当前文件夹里的文件构建到镜像的根目录下
ADD . /

## 指定默认工作目录为根目录（需要把run.sh和生成的结果文件都放在该文件夹下，提交后才能运行）
WORKDIR /

## 镜像启动后统一执行 sh run.sh
CMD ["sh", "run.sh"]Copy
```

1. hello_world.py

里面的内容是本次比赛的程序

```
import json
import csv

file_name = '/tcdata/'

# 第一题，直接写入 Hello world

result = {
"Q1": "Hello world",
"Q2": 0,
"Q3": []
}

# 第二题，求和
list_data = []
sum_int = 0
with open(file_name+"num_list.csv", 'r', encoding='utf-8') as f:
    reader = csv.reader(f)
    for row in reader:
        list_data.append(int(row[0]))
        sum_int += int(row[0])
result['Q2'] = sum_int

# 第三题
result['Q3'] = sorted(list_data, reverse=True)[0:10]

# 保存到 result.json
with open('result.json', 'w', encoding='utf-8') as f:
    json.dump(result, f)Copy
```

1. result.json

一个空文件

1. run.sh

只有一行：

```
python hello_world.py
```

#### 第六步，开启阿里云容器镜像服务

在[教程](https://tianchi.aliyun.com/competition/entrance/231759/tab/174?spm=5176.12282029.0.0.2e85381e2c2PkB)里写得很明白了

[![3yreOK.png](https://s2.ax1x.com/2020/02/29/3yreOK.png)](https://s2.ax1x.com/2020/02/29/3yreOK.png)

需要注意一定要设置密码！在“访问凭证”里，点“管理”即可

#### 第七步，构建镜像与推送

打开powershell，首先要登陆，这个过程在镜像管理的页面里也有，Linux版本要加sudo，但是powershell直接docker开头就好

复制命令

[![3ysENQ.png](https://s2.ax1x.com/2020/02/29/3ysENQ.png)](https://s2.ax1x.com/2020/02/29/3ysENQ.png)

输入密码

然后cd到刚刚创建了四个文件的那个目录下

[![3yse9s.png](https://s2.ax1x.com/2020/02/29/3yse9s.png)](https://s2.ax1x.com/2020/02/29/3yse9s.png)

然后建立

[![3ysAAg.png](https://s2.ax1x.com/2020/02/29/3ysAAg.png)](https://s2.ax1x.com/2020/02/29/3ysAAg.png)

注意：`registry.~~~`是创建仓库的公网地址，用自己仓库地址替换。地址后面的`：1.0`为自己指定的版本号，用于区分每次build的镜像。最后的.是构建镜像的路径，不可以省掉，而且后面有一个.不能省下！

然后推送！

[![3ysVhj.png](https://s2.ax1x.com/2020/02/29/3ysVhj.png)](https://s2.ax1x.com/2020/02/29/3ysVhj.png)

这里不需要写后面的.了

#### 最后一步，验证结果

像是刚才的[手把手教程](https://tianchi.aliyun.com/competition/entrance/231759/tab/174?spm=5176.12282029.0.0.2e85381e2c2PkB)一样

[![3yyuPH.png](https://s2.ax1x.com/2020/02/29/3yyuPH.png)](https://s2.ax1x.com/2020/02/29/3yyuPH.png)

这样提交上去就ok了！

可以查看一下成绩：

[![3yywzn.png](https://s2.ax1x.com/2020/02/29/3yywzn.png)](https://s2.ax1x.com/2020/02/29/3yywzn.png)

（为了博客截图所以提交了两次XD）

好了，这就是docker初战

在这里要吐槽一下tx云，学生优惠的10元/月还真是一点不差

十元一月，只有一个月有优惠，而且给的配置是最低的centos6.9 32位，装不了linux版本的docker

要赚钱还是你会赚啊！企鹅！（咬牙）
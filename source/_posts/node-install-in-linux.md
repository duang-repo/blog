---
title: 在linux下nodejs的安装方法大全
date: 2017-06-11 01:06:46
categories: 杂项
tags:
---
在linux下nodejs的安装方法大全
===

这段时间为了上线测试很多nodejs代码的部署，开始鼓捣云服务器。由于不是很懂运维知识，首先在安装上就踩了很多大坑，于是决定记录下来做个备忘。s

---

首先，我们可以去[nodejs官网](https://nodejs.org/en/download/current/)下载代码。

页面大概长成这样，上面有不同系统的不同位版本的下载，也可以点击上面的连接直接下载源码。

![01.jpg](https://raw.githubusercontent.com/kelekexiao123/markdown-image/master/node-install-linux-01.jpg)

---

第一种方法，直接使用编译好的版本。
---

这是我认为最简单最好用的方法，直接下载官网的可编译版本的压缩包，然后用ftp导入到服务器中。当然你也可以直接在服务器中输入下载链接下载下来。

之后将压缩包文件解压，解压方法根据压缩包类型的不同方法有很多，我随便给一个tar.gz的：

```bash
tar xvf node-v8.1.0-linux-64.tar.gz
```

>其中，node-v8.1.0-linux-64是你的下载文件名，文件名不同这里填写的内容就不同，下面的内容也是一样的，就不一一重复了。

解压之后，我们cd到这个目录执行node，发现已经能直接干了。

```bash
cd node-v8.1.0-linux-64/bin
./node -v
./npm -v
```

但是这样比较麻烦，每次都要进入到这个目录才能执行node和npm。那么怎样在linux里设置全局命令呢？答案是使用ln命令：

```bash
ln -s /root/node-v8.1.0-linux-64/bin/node /usr/local/bin/node
ln -s /root/node-v8.1.0-linux-64/bin/npm /usr/local/bin/npm
```

命令接收两个参数，第一个是你的node文件夹所在的位置，我图省事直接装在root下了，你改了文件位置的话这个地方就写node文件夹所在的位置的父目录即可，第二个是全局指令库位置。

接下来随便跳到一个目录，执行：

```bash
node -v
npm -v
```

是不是就有反应啦。

第二种方式，通过源码编译
---

还是在官网的那个下载页面下载，但是这次选择下载的是Source Code源码，还是原样ftp导入后解压，方法同上。

之后我们进入到源码目录中，进行编译三板斧：

```bash
./configure
make
make install
```

经过一段及其漫长的编译过程后……（make真的漫长，等了得有20分钟，可能是我云服务器配置太差 %>_<%）就能自动的在全局使用node和npm命令啦~

如果你的在服务器上不行的话，加上这一句试试：

```bash
cp /usr/local/bin/node /usr/sbin/
```

这种方式是最推荐使用的，出问题的可能性最小，卸载修改也都很好解决。缺点是……实在是太麻烦了……

第三种方式，通过系统安装apt-get或yum等方式安装nodejs。
---

我写这种方式在这里只是因为确实可以下载并安装。但我这里就不写过程了，因为这种方式下载到的安装包不仅下载慢，下载完成后的包版本还可能不是最新的，安装后还可能出现各种各样的问题，非常的不推荐使用。

（完）
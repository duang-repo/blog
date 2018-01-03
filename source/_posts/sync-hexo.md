---
title: 如何解决github+Hexo的博客多终端同步问题
date: 2017-05-12 15:51:06
categories: "技术栈"
tags: "hexo"
description: "解决github+Hexo的博客多终端同步的思路是将博文内容相关文件放在Github项目中master中，将Hexo配置写博客用的相关文件放在Github项目的hexo分支上，这个是关键，多终端的同步只需要对分支hexo进行操作。"
---

---

本文转载自：[http://blog.csdn.net/Monkey_LZL/article/details/60870891](http://blog.csdn.net/Monkey_LZL/article/details/60870891)

>解决github+Hexo的博客多终端同步的思路是将博文内容相关文件放在Github项目中master中，将Hexo配置写博客用的相关文件放在Github项目的hexo分支上，这个是关键，多终端的同步只需要对分支hexo进行操作。

下面是详细的步骤讲解：

1.准备条件
---

安装了Node.js,Git,Hexo环境
完成Github与本地Hexo的对接
这部分大家可以参考史上[最详细的Hexo博客搭建图文教程](https://xuanwo.org/2015/03/26/hexo-intor/)

配置好这些，就可以捋起袖子大干一场了！

2.在其中一个终端操作，push本地文件夹Hexo中的必要文件到yourname.github.io的hexo分支上
---

在利用Github+Hexo搭建自己的博客时，新建了一个Hexo的文件夹，并进行相关的配置，这部分主要是将这些配置的文件托管到Github项目的分支上，其中只托管部分用于多终端的同步的文件，如完成的效果图所示：

git init  //初始化本地仓库
git add source //将必要的文件依次添加，有些文件夹如npm install产生的node_modules由于路径过长不好处理，所以这里没有用'git add .'命令了，而是依次添加必要文件，如下所示

```bash
git commit -m "Blog Source Hexo"
git branch hexo  //新建hexo分支
git checkout hexo  //切换到hexo分支上
git remote add origin git@github.com:yourname/yourname.github.io.git  //将本地与Github项目对接
git push origin hexo  //push到Github项目的hexo分支上
```

完成之后的效果图为:

![hexo分支](https://raw.githubusercontent.com/kelekexiao123/markdown-image/master/sync-hexo-01.png)

这样你的github项目中就会多出一个Hexo分支，这个就是用于多终端同步关键的部分。

3.另一终端完成clone和push更新
---

此时在另一终端更新博客，只需要将Github的hexo分支clone下来，进行初次的相关配置

```bash
git clone -b hexo git@github.com:yourname/yourname.github.io.git  //将Github中hexo分支clone到本地
cd  yourname.github.io  //切换到刚刚clone的文件夹内
npm install    //注意，这里一定要切换到刚刚clone的文件夹内执行，安装必要的所需组件，不用再init
hexo new post "new blog name"   //新建一个.md文件，并编辑完成自己的博客内容
git add source  //经测试每次只要更新sorcerer中的文件到Github中即可，因为只是新建了一篇新博客
git commit -m "XX"
git push origin hexo  //更新分支
hexo d -g   //push更新完分支之后将自己写的博客对接到自己搭的博客网站上，同时同步了Github中的master
```

4.不同终端间愉快地玩耍
---

在不同的终端已经做完配置，就可以愉快的分享自己更新的博客 
进入自己相应的文件夹

```bash
git pull origin hexo  //先pull完成本地与远端的融合
hexo new post " new blog name"
git add source
git commit -m "XX"
git push origin hexo
hexo d -g
```

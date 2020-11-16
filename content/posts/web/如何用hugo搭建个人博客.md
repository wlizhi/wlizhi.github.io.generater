---
title: "使用hugo搭建个人博客"
date: 2020-04-27T13:06:46+08:00
draft: false
tags: ["Hugo","git","MarkDown"]
categories: ["前端"]
---

### 阅读前你需要了解这些内容：
* git
* MarkDown

使用 Hugo 搭建博客的步骤非常简单。

按照官网 Quick Start 即可快速创建一个站点。此文旨在对官方文档细节补充以及部署到自己的github上。

1. [下载Hugo](https://github.com/gohugoio/hugo/releases)
2. 解压出对应包中的Hugo.exe到一个空目录
3. 将 2 中存放Hugo.exe的目录添加到系统变量"path"中。（不会设置？[点这里](https://www.jb51.net/os/win10/663281.html)）
4. 进入[Hugo官网](https://gohugo.io/) 点击 Quick Start
5. 当下载并设置好Hugo后，就可以直接从Step 2开始操作。
   ```bash
   # 这个 quickstart 请改成你想要的名字,最好对应你github上的名字且全小写
   # 比如：untilthecore.github.io-generator
   # 这个命令会生成 untilthecore.github.io-generator 的文件夹
   # 这个文件夹就是博客站点的生成器，为了方便知道这个文件夹是干什么的，在最后加上 generator，当然 creator 也行
   hugo new site quickstart

   # 那么真正要输入的命令为
   hugo new site untilthecore.github.io-generator
   ```
6. 当得到站点生成器文件夹后，使用 `cd` 命令进入文件夹并依次执行以下命令
   ```bash
    # 为 untilthecore.github.io-generator 初始化 git 仓库
    git init
    # 为站点设置一个主题（这个主题可以修改）
    git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
    # 向配置文件写入 theme = "ananke" 即配置 ananke 主题
    echo 'theme = "ananke"' >> config.toml
   ```
7. 完成以上步骤站点就配置好了，输入以下命令即可创建第一篇博客了
   
   `hugo new posts/my-first-post.md`

8. 通过以上步骤，站点已经创建好了，且写了一篇文章，赶紧通过这个命令启动本地服务器看看你的博客是什么样子吧。

    `hugo server -D`

9. 基础完成了，接下来需要对站点进行一些设置，让它看起来更像是你自己的站点。找到`config.toml` 文件，配置这些内容：
    ```bash
    # 配置域，如果配置到了github上，这里要改为你自己github.io的地址。
    baseURL = "https://example.org/"
    # 配置语言 中文可以改为 "zh-Hans"
    languageCode = "en-us"
    # 站点标签名，浏览器标签上的内容
    title = "My New Hugo Site"
    # 主题设置，有新主题，就把主题名设置到这里
    theme = "ananke"
    ```
10. 本地操作已经完成了，想要将站点部署到github或者自己的云服务器上，请继续往下看。
11. 都配置好后，确保你控制台还处于博客生成器目录下(即"untilthecore.github.io-generator") 输入
    
    `hugo`

     在博客生成器目录下会产生一个'`public/`'的文件夹，将这个文件夹部署到服务器上就可以了。
### 如何在github上部署自己静态博客呢？
1. 在github上创建一个仓库，仓库名为自己github名字的且以"`.github.io`"结尾。（如：[untilthecore.github.io](https://untilthecore.github.io/)）
2. 上面步骤 11 中的 `public` 文件夹还记得吗？首先在博客生成器目录中将它忽略git管理，即放入`.gitignore`中。没有这个文件就创建一个这样的文件，内容保存 "`/public/`" 即可将此文件夹忽略；
3. 进入 `public` 文件夹中，初始化 git 并将这个文件夹上传到步骤1中创建的github仓库中；
4. 在浏览器中进入你自己仓库的`Settings`中，找到图片这个地方看看点击能不能看到博客呢？成功了，祝贺你！没成功，看看有没有哪一步错了？

![图片](../../static/如何用hugo搭建个人博客/githubPaes.jpg)
---
title: "正确使用hugo主题的姿势"
date: 2020-04-27T22:28:15+08:00
draft: false
tags: ["Hugo"]
categories: ["前端"]
---

在《[如何用hugo搭建个人博客中](https://untilthecore.github.io/posts/%E5%A6%82%E4%BD%95%E7%94%A8hugo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/)》已经介绍了简单的主题引入办法，这里就不再赘述了。

现在我们来讲讲如何配置我们下载的新主题！（内容可能不多不全，Hugo的使用还在摸索阶段）

1. 下载主题前要先看主题的Demo，看是否是自己想要的。没有Demo示例给看那就clone下来后跑起来看看是不是中意的，但是有的clone到的主题有可能较精简，怎么办呢？往下看。
2. 选好中意的主题了，那么就一定要看作者的配置介绍，这个配置介绍一般都是介绍如何配置`config.toml`，但这个介绍可能不够全，那么就要自己看clone下来的主题中这个文件夹`exampleSite`中的`config.toml`。有的主题还有`full-config.toml`,这个内容更全面。以我使用的主题为例，路径结构为：`themes\hello-friend-ng\exampleSite\config.toml`
3. 接下来我将以[我使用的主题](https://themes.gohugo.io/hugo-theme-hello-friend-ng/)为例，介绍`config.toml`配置内容
   ```bash
    [[menu.main]]
      identifier = "blog"
      name       = "Blogs"
      url        = "/posts"
    [[menu.main]]
      name       = "Categories"
      identifier = "categories"
      url        = "/categories/"
    [[menu.main]]
      identifier = "tags"
      name       = "Tags"
      url        = "/tags/"
    [[menu.main]]
      identifier = "about"
      name       = "About"
      url        = "about/"
   ```
   有`[[menu.main]]`即配置导航菜单，对应的地方如图：

   ![导航菜单图](../../static/正确使用hugo主题的姿势/导航菜单图.png)

    ```bash
    [[params.social]]
    name = "email"
    url  = "mailto:untilthecore@gmail.com"

    [[params.social]]
    name = "github"
    url  = "https://github.com/UntilTheCore"
    ```
    有`[[params.social]]`即配置图标链接，对应地方如图：

    ![图标链接图](../../static/正确使用hugo主题的姿势/图标链接.jpg)

    ```bash
    baseurl      = "https://untilthecore.github.io/"
    title        = "My Blog"
    languageCode = "zh-Hans"
    theme        = "hello-friend-ng"
    ```
    - baseurl：配置站点基址，如果要放在github上或者自己的云服务器上，请正确配置地址，否则出了能访问主页，其他地方都是 404
    - title：设置标题
    - languageCode：设置语言
    - theme：设置主题
    
    其他的小地方的配置有注释，配合本地服务器可以方面查看修改后的效果。
4. 第一次使用时，可能会好奇我们的 `.md` 文件是怎么产生标签（Tags）和分类（Categories）的？这就是通过使用`hugo new posts/xxxxxx.md`创建博客文件时，hugo自动为我们在文件头部生成了这些内容：
   
   ```bash
   title: "正确使用hugo主题的姿势"
   date: 2020-04-27T22:28:15+08:00
   draft: false
   tags: ["Hugo"]
   categories: ["教程"]
   ```
   - title：文章标题；
   - data：文章被创建的事件
   - draft：是否草稿内容，如果为true，将不显示在博客站点中。
   - tags：为这篇文章创建标签，是数组形式`["1","2","3"]`
   - categories：为这篇文章创建分类，是数组形式`["1","2","3"]`
  
    由于在文章这里设置了`tags`和`categories`,所以我们可以在导航菜单中看到标签和文章分类信息。

在`.md`文件中，还可以设置更多其他的内容，比如设置版权信息等。有需要的话可以自行了解，我的经验就是多下几个主题，看看这些作者是怎么配置的。如果你有更多更好的信息，欢迎来信告诉我

创建博客后如何配置主题的基础内容介绍完毕，配置好这些就可以愉快地写博客啦！如果想配置更多更强大的内容，可以多多翻看[Hugo官方文档](https://gohugo.io/documentation/)。

#### 写博客时要注意的点：

可能在posts中使用MarkDown插入图片时好好的，但是上传到github上后图片就不显示了呢？这是由于我们写文章时是在站点生成器的环境中，而上传的代码是在public中，为了保证上传后依然能正确显示，我使用的技巧是在public文件夹中新建一个static的文件夹用来保存我们文章的图片，且每一文章有对应的文件夹去保存图片，如图：

![文章图片的保存方式](../../static/正确使用hugo主题的姿势/文章图片的保存方式.jpg)

鉴于此，我们在`posts/`下写文章时使用的图片地址要以`public/posts/文章标题/index.html`为基准来找到`public\static`目录。一段示例：

    ../../static/正确使用hugo主题的姿势/文章图片的保存方式.jpg


文章有误或不足之处，欢迎指正！谢谢！

### 本文由UntilTheCore原创，转载请说明作者和出处！
---
title: "HTML入门笔记1"
date: 2020-04-28T18:12:06+08:00
draft: false
tags: ["HTML", "入门"]
categories: ["前端"]
---

## 1.HTML 是怎么产生的？

> ##### Tim Berners-Lee -- 万维网的创建者

探寻互联网的初始，那么就必然要了解一下[李老爷子](https://baike.baidu.com/item/%E8%92%82%E5%A7%86%C2%B7%E4%BC%AF%E7%BA%B3%E6%96%AF%C2%B7%E6%9D%8E/8868412?fromtitle=Tim%20Berners-Lee&fromid=1836386&fr=aladdin)本人，这位牛人为如今精彩纷呈的互联网世界打下了坚实的地基。1989 年他开发了世上第一个浏览器和第一个服务器，由此种下了互联网世界的种子，而且他发明的 WWW、URL、HTTP 一直延续至今。

那这个东西到底是怎么产生的呢？1、因为一杯咖啡，这让我联想到了 java；2、为了可以上网冲浪；

## 2.HTML 怎么快速开始

现代的编辑器配合 Emmet 语法可以快速创建一个 HTML 的骨架，虽然这节省了我们书写骨架代码的时间，但还是有必要了解一下这些内容是什么。
![HTML骨架解读](https://img-blog.csdnimg.cn/20200428163933267.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L1VudGlsVGhlQ29yZQ==,size_16,color_FFFFFF,t_70)
在 vscode 中创建一个 HTML 的文件，使用 Emmet 语法输入`! + tab键`即可快速生成 html 骨架。

## 3.HTML 常用的章节标签有哪些？

| 章节标签 | 介绍                                                                                                         |
| -------- | ------------------------------------------------------------------------------------------------------------ |
| h1~h6    | 文章内容标题                                                                                                 |
| section  | 相对于 article 元素更加广泛，每个区块都可以使用，比如页面里的导航菜单、文章正文、文章的评论等。              |
| article  | 代表文档、页面或应用程序中独立的、完整的、可以独自被外部引用的内容。它可以是一篇博客、一篇帖子、一段用户评论 |
| p        | 段落                                                                                                         |
| header   | 头部                                                                                                         |
| footer   | 页脚                                                                                                         |
| main     | 主要内容。内容主体区域，放在在 header 和 footer 中间                                                         |
| aside    | 旁支内容                                                                                                     |
| div      | 划分                                                                                                         |

## 4.HTML 常用的内容标签有哪些？

| 内容标签    | 介绍                                                                  |
| ----------- | --------------------------------------------------------------------- |
| ol + li     | ordered list + list item。无序列表                                    |
| ul + li     | unordered list + list item。有序列表                                  |
| dl+ dd + dt | description list + term + data。自定义列表                            |
| pre         | 被 pre 包裹的内容以原始内容输出。比如内容有多个空格以及换行都正常显示 |
| hr          | 分隔线                                                                |
| br          | 换行                                                                  |
| a           | 超链接标签                                                            |
| em          | 语气上强调                                                            |
| strong      | 表示内容重要                                                          |
| code        | 内容字体等宽。一般包裹代码块                                          |
| quote       | 引用。行内引用                                                        |
| blockquote  | 块级引用。被包裹的内容与上一行内容有一个缩进                          |

## 5.HTML 的全局属性有哪些？

全局属性即所有标签都会有的属性。

| 全局属性        | 介绍                                                                                                                                                     |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| class           | 类。给元素取一个类名，一般配合 css 设置样式                                                                                                              |
| id              | 全局唯一 id，一般配合 js 使用，但是由于多处使用了浏览器也不报错，所以慎用                                                                                |
| style           | 内联样式                                                                                                                                                 |
| title           | 鼠标移到标签上会在鼠标旁显示 title 的值                                                                                                                  |
| hidden          | 控制标签的隐藏                                                                                                                                           |
| tabindex        | 如果元素需要被键盘 tab 键选中，可以设置值。值 < 0，表示不被 tab 选中；值 == 0，表示最后被选中；值 > 0 表示按值大小顺序选中                               |
| contenteditable | 使标签可被直接编辑。想要更直观看到这个属性的效果可以[点击这里](https://jsbin.com/varinuz/2/edit?html,output)修改样式试试看，或者复制下面的代码运行一下。 |

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    <title>JS Bin</title>
  </head>
  <body>
    <!-- 将style放入body并设置display为block再开启元素可编辑即可在网页上编辑页面样式 -->
    <style style="display:block;" contenteditable>
      header {
        background-color: pink;
      }
    </style>
    <header>我是头部</header>
    <section contenteditable>我是章节</section>
    <footer>我是页脚</footer>
  </body>
</html>
```

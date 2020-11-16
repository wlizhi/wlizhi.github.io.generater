---
title: "HTML入门之HTML常用标签"
date: 2020-04-29T11:00:02+08:00
draft: false
toc: true
tags: ["HTML", "入门"]
categories: ["前端"]
---

| 标签                                                                     | 常用属性                                                                                        | 作用                                                                |
| ------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| [a](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/a)         | href \ target                                                                                   | 1、跳转外部页面；2、跳转内部锚点；3、跳转到邮箱或电话               |
| [img](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/img)     | src \ alt \ width \ height                                                                      | 发出 get 请求，展示一张图片                                         |
| [form](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/form)   | action \ method                                                                                 | 表单。发送 get 或 post 请求，然后刷新页面                           |
| [input](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/input) | type [ "text" , "password" , "file"，"submit"，"hidden"等 ] \ checked \ value \ disabled \ name | 让用户输入内容。通过 type 确定不同的 input 内容，通常搭配 form 使用 |
| [table](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table) | -                                                                                               | 表格。使用相对较少                                                  |

## 1. a 标签

### 属性[ href ]

`[href]`可设置以下内容的值：

- 网址 
  - [https://untilthecore.github.io/](https://untilthecore.github.io/) 
  - [http://untilthecore.github.io/](https://untilthecore.github.io/) 
  - [//untilthecore.github.io/](https://untilthecore.github.io/) (推荐用这种，它会自动寻找 https 或者 http 协议的网址)
- 路径 - 绝对路径：/a/b/c （以地址栏地址为基准寻找） - 相对路径：a/b/c 、 ./a/b/c (以当前文件为基准寻找)
- 伪协议 - `"javascript:代码;"`：代码部分可以写代码，这样点击会产生对应代码的效果。或者什么都不写，就是单纯的`"javascript:;"`，这样可以阻止 a 标签的默认跳转行为。 ； - `"mailto:邮箱地址"`：值为这个，点击时会调用默认邮件程序，可以发送邮件到填写的邮箱地址； - `"tel:手机号"`：值为这个，如果是手机端会调用手机默认拨号程序并准备拨向填写的手机号；
- id
  锚点。如果 a 标签的 href 属性值为某个元素的 id 值，点击时会跳到这个元素的位置。举个例子：

```html
<!-- 忽略HTML骨架 -->
<!-- 假如有一个元素p，且 id 为 aaa -->
<p id="aaa"></p>
<!-- 假如有一个 a 标签，它的 href="#aaa",且这个 a 标签离 上述 p 间隔100个p标签的距离 -->
<a href="#aaa">跳向aaa</a>
```

注：href 可以不设置值，但是点击时会造成页面刷新，有可能有的小伙伴认为可以填写上`"#"`来避免刷新，但是使用这种方式虽然不刷新，但是如果页面很高，而且这个 a 标签在很下面，会导致页面回到顶部。为了既防止页面刷新又防止回到页面顶部，可以使用`"javascript:;"`

### 属性[ target ]

`"[target]"`可以设置以下内容的值：

- "\_blank"：打开链接到一个新的浏览器标签页上。
- "self"：默认值。打开链接在当前页面上。
- "\_top"：打开链接到最顶级的页面，通常和 iframe 使用。不过 iframe 不怎么用了，仅做了解。
- "parent"：打开链接到父级页面，通常和 iframe 使用。比如 html > iframe a > iframe b，iframe b 中 a 的 target 设置为 parent，那么打开链接时会在 iframe a 中打开。同上，仅做了解。

## 2. img 标签

### 属性[ src ]

`"[src]"`值为图片的地址，可以使网上图片的超链接，也可以是本地的图片路径。

### 属性[ alt ]

`"[alt]"`的值为一段关于图片的描述，在图片加载失败或者网速过慢还没有加载出来时显示给用户看的内容。

### 属性[ width \ height ]

`"[width] 和 [height]"`：设置图片的宽和高。

### 事件[ onload ]

`"[onload]"`：在图片加载完成后触发的事件

### 事件[ onerror]

`"[onerror]"`：在图片加载失败后触发的事件。应用场景：比如用户访问某个图片失败了，通过这个事件可以替换 img 的 src 为一张默认图给用户看，用来保证一些用户体验性。

### 响应式

`"[max-width]"`：为了保持图片在不同设备上显示效果一致，可以在 css 中设置 img 的`max-width`

```html
<style>
  img {
    max-width: 100%;
  }
</style>
```

而为什么要使用`"max-width"`而不是`"width"`呢？最重要的概念是**保证图片不被拉伸**，使用`"max-width"`可以保证图片小于屏幕宽度的时候不被拉伸到和屏幕一样宽，只到自己图片大小，从而避免造成看起来非常马赛克。如果还是不太明白可以写代码来帮助理解。

## 3.form 标签

### 属性[ action ]

`"[action]"`：一个处理此表单信息的程序所在的 URL。

### 属性[ method ]

`"[method ]"`：表单的提交方式，值有：

- post：表单数据会包含在表单体内然后发送给服务器。
- get：表单数据会附加在 action 属性的 URI 中，并以 '?' 作为分隔符，然后这样得到的 URI 再发送给服务器。如果要发送用户的账号密码等个人信息，请使用 post 方式。

### 事件 [onsubmit]

表单提交时会触发 onsubmit 事件，有时候不想要表单默认的提交行为，我们可以给表单添加 id 然后通过 js 捕获 submit 事件并在事件处理函数中 return false 来阻止默认提交行为。

## 4.input 标签

### 属性[ type ]

`"[input]"`的 type 属性不同值可以产生各式各样的 input 内容，比如单选框 radio 、复选框 checkbox、颜色选择面板 color 等，大多数的类型值都是比较好理解的，但是这个 `"hidden"` 是个什么意思且具体用法是什么对于初学者来说有点懵。
为什么要有一个 type 属性值是`"hidden"`呢？这是因为有时候用户查询得到的数据返回时在地址栏上，而且这次查询得到的数据还要继续用来下一次查询，这时总不可能让用户自己去复制粘贴地址栏上的参数数据吧？所以为了避免这种情况，就需要`"hidden"`上场了。我们可以在需要用来保存上一次查询结果的地方增加一个`<input type="hidden" value="">`在我们得到返回结果后通过 js 将结果设置到这个 input 的 value 上，这样在我们下一次需要的时候就可以直接通过 value 去取到这个值了。
这种操作表单的方式被称作：`隐藏域`

### 属性[ name ]

`"[name]"`属性作用有这些：

- 给 radio 和 checkbox 归类；
- 表单提交时传给后台的表单项的名字，它与属性 `"value"`组成键值对发送到后端，`"name"`是键。

### 属性[ value ]

`"[value]"`属性与`"name"`属性组成键值对发送到后端，`value`是值。

### 事件[ onchange ]

`"[onchange]"`：input 内容发生改变时触发的事件，比如类型为 text 的 input 有输入产生、类型为 select 的 input 选中了某个选项等。

### 事件[ onfocus]

`"[onfocus]"`：input 产生焦点时触发的事件。比如有一个类型为 text 的 input，我们鼠标点击了这个 input 时产生`"onfocus"`事件。

### 事件[ onblur ]

`"[onblur ]"`：input 失去焦点时触发的事件。比如一个类型为 text 的 input，内容输入完成后鼠标点击了非这个 input 区域时产生`"onblur"`事件。

## 5.table 标签

`"[table]"`内容比较简单，主要就是练习一个熟练度。table 本身用的地方不是很多，但可以用来进行一些比较特殊表格布局。
使用`table`注意的点：

- 使用 css 设置`border-collapse:collapse`和`border-spacing: 0`，这样可以清除表格默认样式，使表格更好看。
- 不要在标签上设置表格样式，请在 css 中设置 table 的样式，这是官方说的。

### 熟能生巧

[一个简单表格的练习](https://codepen.io/untilthecore/pen/zYvzgyB)

打不开就看这里吧：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      th,
      td {
        border: 1px solid #ccc;
        text-align: center;
      }
      table {
        width: 300px;
        /* 设置单元格距离和单元格边框合并 */
        border-collapse: collapse;
        border-spacing: 0;
      }
    </style>
  </head>
  <body>
    <table>
      <thead>
        <tr>
          <th></th>
          <th>价格</th>
          <th>分类</th>
          <th>日期</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>商品1</th>
          <td>100</td>
          <td>分类1</td>
          <td>2020-4-29</td>
        </tr>
        <tr>
          <th>商品2</th>
          <td>100</td>
          <td>分类2</td>
          <td>2020-4-29</td>
        </tr>
        <tr>
          <th>商品3</th>
          <td>100</td>
          <td>分类3</td>
          <td>2020-4-29</td>
        </tr>
      </tbody>
      <tfoot>
        <tr>
          <th>价格总计</th>
          <!-- colspan 让这一列占 3 列的宽度 -->
          <td colspan="3">300</td>
        </tr>
      </tfoot>
    </table>
  </body>
</html>
```

学习使人充盈，分享使人进步！

此文为 UntilTheCore 原创，转载请说明作者和出处！

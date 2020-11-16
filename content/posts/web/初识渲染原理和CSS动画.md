---
title: "初识渲染原理和CSS动画"
date: 2020-05-02T17:33:32+08:00
draft: false
tags: ["css", "入门"]
categories: ["前端"]
---

## 1、浏览器渲染原理

浏览器到底是怎么将一个 html 文件渲染成生动的页面的？话不多说，直接看图

![初识渲染原理-渲染流程](https://i.loli.net/2020/05/06/DxKJ6HQ7aBRohAP.png)

以图中的过程，浏览器的渲染过程大致就是：

1. 通过 HTML 解析将 HTML 文件解析为 DOM 树;
2. 通过 CSS 解析将 CSS 文件解析出样式规则（CSSOM 树）；
3. 通过合并 DOM 树和 CSSOM 树将生成渲染树；
4. 依赖渲染树开始布局（文档流、盒模型、确定位置和尺寸计算）；
5. 知道了哪些节点可见、它们的计算样式以及几何信息，将渲染树中的每个节点转换成屏幕上的实际像素即开始绘制。把边框、文字颜色、阴影等绘制出来。
6. 最后将合成好的页面展示出来。

**延伸：**

1. 层叠样式的由来

还是先看图：

![初识渲染原理-CSS树png](https://i.loli.net/2020/05/06/4XZaoerMtkIinh1.png)

为页面上的任何对象计算最后一组样式时，浏览器都会先从适用于该节点的最通用规则开始（例如，如果该节点是 body 元素的子项，则应用所有 body 样式）这种行为被称作为“样式继承”，那么为了不想要继承过来的样式，那么就需要自己单独写样式来覆盖原来的样式，即层叠。而不同的浏览器提供了不同的默认样式（“User Agent 样式”），这让我们一套代码却产生了五花八门的样式，这不符合设计需求。所以又通常要编写一个全局样式（reset.css / normalize.css）来清除默认样式，让代码在不同浏览器上产生一致的效果。

2. "display：none"的元素是否会被渲染？

答案是否。 同为显示和隐藏的一个属性是"visibility",visibility: hidden 与 display: none 是不一样的。前者隐藏元素，但元素仍占据着布局空间（即将其渲染成一个空框），而后者 (display: none) 将元素从渲染树中完全移除，元素既不可见，也不是布局的组成部分。

3. 回流和重绘

**回流(reflow)：**
回流或称为 layout 重排。当浏览器发现某个部分发生了点变化影响了布局，需要倒回去重新渲染。reflow 会从这个 html 的根节点开始往下递归，依次计算所有的结点几何尺寸和位置。reflow 几乎是无法避免的。现在界面上流行的一些效果，比如树状目录的折叠、展开（实质上是元素的显示与隐藏）等，都将引起浏览器的 reflow。鼠标滑过、点击等只要这些行为引起了页面上某些元素的占位面积、定位方式、边距等属性的变化，都会引起它内部、周围甚至整个页面的重新渲染。通常我们都无法预估浏览器到底会 reflow 哪一部分的代码，它们都彼此相互影响着。

**重绘(repaint)：**
改变某个元素的背景色、文字颜色、边框颜色等等不影响它周围或内部布局的属性时，屏幕的一部分要重画，但是元素的几何尺寸没有变。

每次 Reflow，Repaint 后浏览器还需要合并渲染层并输出到屏幕上。所有的这些都会是动画卡顿的原因。
Reflow 的成本比 Repaint 的成本高得多的多。一个结点的 Reflow 很有可能导致子结点，甚至父点以及同级结点的 Reflow 。在一些高性能的电脑上也许还没什么，但是如果 Reflow 发生在手机上，那么这个过程是延慢加载和耗电的。可以在 csstrigger 上查找某个 css 属性会触发什么事件。

**reflow 与 repaint 的时机：**
display:none 会触发 reflow，而 visibility:hidden 只会触发 repaint，因为没有发生位置变化。
有些情况下，比如修改了元素的样式，浏览器并不会立刻 reflow 或 repaint 一次，而是会把这样的操作积攒一批，然后做一次 reflow，这又叫异步 reflow 或增量异步 reflow。
有些情况下，比如 resize 窗口，改变了页面默认的字体等。对于这些操作，浏览器会马上进行 reflow。

下面看谷歌开发者网站上的一张图

![初识渲染原理-渲染时机](https://i.loli.net/2020/05/06/pKvlcD4wz8ZVfWU.png)

如果更改一个既不要布局也不要绘制的属性，则浏览器将跳到只执行合成。

这个最后的版本开销最小，最适合于应用生命周期中的高压力点，例如动画或滚动。
继续深入将涉及到浏览器渲染优化，这里不再做展开。想知道哪些 CSS 属性会触发上面 3 中中的哪一个，可以查看这个网站[CSS 触发器](https://csstriggers.com/)。

使用不同的方式改变元素状态的最终显示过程是什么样的呢？

1. 使用 js 调整元素位置 => 触发上图中的 Layout+Paing+Composite
2. 使用 CSS 调整元素背景色 => 触发上图 Paint 和 Composite
3. 使用 transform 调整元素位置 => 触发上图 Composite

是否发现使用 transform 调整元素竟然只触发了 Composite，比 js 少了两个步骤。很明显，使用 transform 可以提高页面渲染性能，而且用它还可以制作动画，下面来看看动画该怎么做吧。

## 2、CSS 动画的两种做法（transition 和 animation）

说动画前先深入了解一下"transform"属性。一般常用来给元素改变位置（translate），旋转（rotate），缩放（scale）。如果还需要对元素设置 3d 样式的话，就要给被设置元素的父级元素或者直接给 body 设置 perspective 属性。

要给元素进行变换前，还要清楚一个知识点-坐标系。我们浏览器以及盒模型的坐标原点都为左上角

![初识渲染原理-坐标](https://i.loli.net/2020/05/06/RlmXuZ8baDqKOUC.jpg)

当元素发生旋转时这个坐标轴的朝向也将发生改变。注意这一点，有时候就是没考虑好坐标轴的问题造成了布局错乱。

![初识渲染原理-旋转后坐标](https://i.loli.net/2020/05/06/7fklsnZcKjg4huM.jpg)

了解了坐标系，使用"transform:translate"的时候就更得心应手了。

**transition：**
使用 transform 是将元素从上一个状态转变为 transform 后的状态，直接设置这个属性后我们在浏览器中看到的就是 transform 后的元素了，为了让元素在发生变化时有一个过渡的效果，此时就可以用上"transition"啦，配合上":hover"等伪类选择器就可以实现简单的一次性动画效果啦。了解更多关于 transition 的内容可以看[MDN transition](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transition)

**animation：**
transition 只能制作一次性的动画，想要制作类似这样效果的动画怎么办呢？

![初识渲染原理-跳动的心](https://i.loli.net/2020/05/06/2YbXW7MjDLHpPrm.gif)

使用[animation](https://developer.mozilla.org/zh-CN/docs/Web/CSS/animation)配合上"@keyframes name{}"就可以实现了。属性使用没什么技巧，就是多练。详情看[MDN @keyframes](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@keyframes)
。知道了 keyframes 怎么写了，那 animation 又该写到哪里呢？和 transition 一样，将这个属性写到要被实现动画元素的**本身上！**

说不如做，动手做一个跳动的心吧，源码[跳动的心](https://codepen.io/untilthecore/pen/rNOGWYM)

### transform 总结：

1. inline 元素不支持 transform，需要先转变为 block；
2. translate(-50%,-50%)可做绝对定位元素的居中；
3. scale 会造成模糊或边界变粗，酌情使用；
4. 属性组合用：transform:scale(1.5) translate(-100%,-100%)
5. 善用搜索引擎，不明白就看 MDN；

上述内容参考 MDN 以及谷歌 web 相关文档。

- [googleDevelopers-渲染性能 https://developers.google.com/web/fundamentals/performance/rendering](https://developers.google.com/web/fundamentals/performance/rendering)
- [googleDevelopers-关键渲染路径 https://developers.google.com/web/fundamentals/performance/critical-rendering-path](https://developers.google.com/web/fundamentals/performance/critical-rendering-path)

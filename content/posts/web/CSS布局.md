---
title: "CSS布局"
date: 2020-05-01T15:15:09+08:00
draft: false
toc: true
tags: ["css", "入门"]
categories: ["前端"]
---
## float布局
### 常见问题
**原理**：在父盒子浮动元素的最后加上一个盒子，并给这个盒子设置属性"clear:both"，这样就可以实现最基础的清除浮动了。
但是这样有个缺点，就是总要去手动增加一个html元素，然后为这个元素设置css样式，太麻烦。什么方式可以避免呢？那就是使用css伪元素。对css伪元素不太了解的话，可以[看这里](https://www.cnblogs.com/wonyun/p/5807191.html)。
使用伪元素的方式清除浮动，则只需要为浮动盒子的父盒子添加一个类样式。类样式代码如下：
```css
/* 不兼容ie低版本 */
.clearfix {
  content:"";
  display:block;
  clear:both;
}

/* 兼容ie低版本 */
/* after伪元素清除法 */
.clearfix:after {
  content: "";
  display: block;
  height: 0;
  clear: both;
  visibility: hidden;
}
.clearfix: {
  *zoom: 1; /*为i6/i7老式浏览器准备*/
}
/* 双伪元素清除法 */
.clearfix:before,
.clearfix:after {
  content: "";
  display: table;
}
.clearfix:after {
  clear: both;
}
.clearfix: {
  *zoom: 1; /*为i6/i7老式浏览器准备*/
}
```
#### 2. 图片logo与底部边框有间隙
给img标签添设置属性"vertical-align: middle;"值可以是这个属性的其他值。
#### 3. 边框影响了布局
将border属性改为使用outline属性。同样都可以显示边框，但outline不影响布局。
#### 4. 块元素居中显示
在pc上布局时，有时候要对一个固定宽度的块元素设置居中显示，可以使用"margin:0 auto;"的方式，但是这并不好，因为假如这个要居中显示的元素就需要一个上外边距，那么使用"margin:0 auto;"会破坏布局。比较好的方式是为元素设置以下两种属性：
```css
.centerbox {
  margin-left:auto;
  margin-right:auto;
}
```
注：在css中，非特殊情况，布局时的属性设置最好是只影响到被设置的元素本身，不要让代码影响到其他的布局结果。即改写的代码不要少，但不该写的一定不要多。

#### 5.平均布局时会用到的小技巧

![](https://user-gold-cdn.xitu.io/2020/5/1/171ceff5d16dd830?w=649&h=353&f=png&s=3451)
如图，遇到需要对黑盒子内橙色盒子平均布局时，因为我们需要设置"margin-right:xxpx;"会遇到最后一个盒子被挤下去的情况。这是由于每个盒子都"margin-right"使得最后一个橙色盒子装不下了。此时只需在这些需要被平均布局的盒子外再嵌套一个看不见的蓝色盒子，并给它设置一个最后一个橙色盒子所需的"margin-right"大小值就可以实现正常布局了，即图中蓝色盒子超过部分大小。这种布局方式称为负margin。
## flex布局
### 常用属性
| 属性                                   | 设置位置 | 描述                         |
| -------------------------------------- | -------- | ---------------------------- |
| display:flex                           | 父元素   | 将布局设置为flex布局         |
| flex-direction:row(默认)/column        | 父元素   | 设置主轴方向                 |
| flex-wrap:wrap/nowrap(默认)            | 父元素   | 设置子元素是否换行           |
| justify-content:center / space-between | 父元素   | 子元素在主轴上的显示方式     |
| align-items:center                     | 父元素   | 设置子元素在侧轴上的显示方式 |
了解详细的flex，[看这里](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
### 注意的点
1. 尽量不要把宽高写死。即不要直接设置为固定px单位的值。可以使用"百分比"，"vw"，"vh"、或者设置"max-width/min-width"。
2. 平均布局时适用上述float中的技巧。
3. flex和margin-xxx:auto;配合会有意外效果。

## grid布局
grid布局是未来的发展趋势，它简单、强大、易懂。

[学习grid布局](http://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

[通过一个游戏练习使用grid](https://cssgridgarden.com/#zh-cn)

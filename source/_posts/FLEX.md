---
title: FLEX
date: 2022-07-09 22:22:22
description: Flexible Box 弹性布局
categories:
- CSS
tags:
- CSS
- FLEX
---

## Flexible Box 弹性布局

```css
/* 容器盒子 */
.box{
    display: flex;
}

/* 行内元素 */
.box{
    display: inline-flex;
}

/* Webkit 内核浏览器 Safari */
.box{
    display: -webkit-flex;
    display: flex;
}
```
采用`Flex`布局的元素，称为`Flex`容器，子元素`Flex Item`自动为容器的子项

容器默认存在两根轴：水平的主轴`main axis`和垂直的交叉轴`cross axis`，主轴的开始位置（与边框的交叉点)叫做`main start`，结束位置叫做`main end`，交叉轴的开始位置叫做`cross start`，结束位置叫做`cross end`

容器中元素默认沿主轴排列，子项占据的主轴空间叫做`main size`、交叉轴空间叫做 `cross size`

## 容器属性
### Flex-Direction 属性
子项的排列（主轴）**方向**：
```css
.box {
    flex-direction: row | row-reverse | column | column-reverse;
}
```
- `row`**（默认值）：**水平方向，从左到右
- `row-reverse`**：**水平方向，从右到左
- `column`**：**垂直方向，从上到下
- `column-reverse`**：**垂直方向，从下到上
### Flex-Wrap 属性
关于在一行中的排列**换行**问题：
```css
.box{
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```
- `nowrap`**（默认）：**不换行
- `wrap`**：**换行，第一行在上方
- `wrap-reverse`**：**换行，第一行在下方
### Flex-Flow 属性
`flex-flow`：`flex-direction` 和`flex-wrap`的**简写**形式，**默认值为**`row nowrap`

```css
.box {
    flex-flow: <flex-direction> <flex-wrap>;
}
```
### Justify-Content 属性
定义了项目在**主轴**上的**对齐**方式
```css
.box {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```
具体对齐方式与轴的方向有关。假设主轴为从左到右：
- `flex-start`**（默认值）：**左对齐
- `flex-end`**：**右对齐
- `center`**：**居中
- `space-between`**：**两端对齐，项目之间的间隔都相等
- `space-around`**：**每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍
- `space-evenly`**：**两侧与中间间距都相等
### Align-Items 属性
定义**单轴线**在**交叉轴**上如何**对齐**，多轴不起作用
```css
.box {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```
具体的对齐方式与交叉轴的方向有关，假设交叉轴从上到下：
- `flex-start`**：**起点对齐
- `flex-end`**：**终点对齐
- `center`**：**中点对齐
- `baseline`**：**子项的第一行文字的基线对齐
- `stretch`**（默认值）：**如果子项未设置高度或设为`auto`，将占满整个容器
### Align-Content 属性
定义了**多轴线**在**交叉轴**的**对齐**方式（比如说好几行），单轴不起作用
```css
.box {
  align-content: flex-start | 
                 flex-end | 
                 center | 
                 space-between | 
                 space-around | 
                 stretch;
}
```
- `flex-start`：与交叉轴的起点对齐
- `flex-end`：与交叉轴的终点对齐
- `center`：与交叉轴的中点对齐
- `space-between`：与交叉轴两端对齐，轴线之间的间隔平均分布
- `space-around`：每根轴线两侧的间隔都相等。所以，轴线之间的间隔比轴线与边框的间隔大一倍
- `stretch`（默认值）：轴线占满整个交叉轴
## 子项属性
### Order 属性
定义项目的**排列顺序**：数值越小，排列越靠前，默认为`0`
```css
.item {
    order: <integer>;
}
```
### Flex-Grow 属性
定义项目的**放大比例**，默认为`0`，即如果存在剩余空间，也不放大
```css
.item {
    flex-grow: <number>;
}
```
如果所有子项的`flex-grow`属性都为`1`，则它们将等分剩余空间（如果有的话）
如果一个子项的`flex-grow` 属性为`2`，其他项目为`1`，则前者占据的剩余空间将比其他项多一倍
### Flex-Shrink 属性
定义了项目的**缩小比例**，默认为`1`，即如果空间不足，该项目将缩小
```css
.item {
    flex-shrink: <number>;
}
```
如果所有子项的`flex-shrink`属性都为`1`，当空间不足时，都将等比例缩小
如果一个子项的`flex-shrink`属性为`0`，其他项目都为`1`，则空间不足时，前者不缩小
### Flex-Basis 属性
定义了在**分配多余空间之前**，**子项占据的主轴空间**`main size`，浏览器根据这个属性，计算主轴是否有多余空间，默认值为`auto`，即子项的本来大小
```css
.item {
    flex-basis: <length> | auto;
}
```
可设为跟`width`或`height`属性一样的值`?px`，则项目将占据固定空间
### Flex 属性
`flex-grow`＋`flex-shrink`＋`flex-basis`的**简写**，默认值为`0` `1` `auto`，后两个属性可选
```css
.item {
    flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}
```
该属性有两个快捷值：`auto`：`1 1 auto`和 `none`：`0 0 auto`
建议**优先使用**这个属性，而不是单独写三个分离的属性，因为浏览器会推算相关值

### Align-Self 属性
允许**单个子项**有与其他子项**不一样的对齐方式**，可覆盖`align-items`属性，默认值为`auto`，表示继承父元素的`align-items`属性，如果没有父元素，则等同于`stretch`
```css
.item {
    align-self: auto | flex-start | flex-end | center | baseline | stretch;
}
```

**内容来自：**[**FLEX布局教程**](https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)  l  [**FLEX**](https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Flexible_Box_Layout/Basic_Concepts_of_Flexbox)

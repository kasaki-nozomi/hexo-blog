---
title: 视差滚动
date: 2022-09-03 18:40:22
description: 视差滚动 Parallax Scrolling
categories:
- CSS
tags:
- CSS
---

**<font size = 6 color = dd00dd>视差滚动</font>**

***

**视差滚动 `Parallax Scrolling`** 指多层背景以不同的速度移动，形成立体的运动效果

实现方式

- `background-attachment`
- `transform: translate3D`
## Background-Attachment
**`background-attachment`：设置背景图像是否固定或者随着页面的其余部分滚动**

- **`scroll`** 默认，背景图片随**页面**的滚动而滚动
- **<font color = dd00dd>`fixed` 背景图片不随着页面的滚动而滚动</font>**
- **`local`** 背景图片随着**元素内容**的滚动而滚动
- **`initial`** 设置该属性的默认值
- **`inherit`** 从父元素继承

```html
<html>
<head>
<style>
div {
    height: 100vh;
    line-height: 100vh;
    font-size: 20vh;
    text-align: center;
    color: white;
    background: black;
    background-attachment: fixed;
    background-size: cover;
    background-position: center center;
}
.img-two {
    background-image: 
      url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/kkiana.png);
}
.img-four {
    background-image: 
      url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/tomako.png);
}
.image-six {
    background-image: 
      url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/unknow.png);
}
</style>
</head>
<body>
    <div>ONE</div>
    <div class="img-two">TWO</div>
    <div>THREE</div>
    <div class="img-four">FOUR</div>
    <div>FIVE</div>
    <div class="image-six">SIX</div>
</body>
</html>
```
## Transform: translate3D
两个属性：
- **Transform：**对元素进行变换 `2d/3d`，平移 `translate`、旋转 `rotate`、缩放 `scale` 等
- **<font color="#ddoodd">Perspective：</font>**元素涉及 `3d` 变换时，`perspective` 可以定义看到的 `3D` 立体效果，即空间感

容器设置 `transform-style: preserve-3d` 和 ` perspective: ?px`，使子元素位于 `3D` 空间中
子元素设置不同的 `transform: translateZ()`，不同元素在 `3D` `Z` 轴方向距离屏幕距离不一样

```html
<html>
<head>
<style>
body {
    width: 100%;
    height: 100%;
    overflow: hidden;
}
.wrapper {
    width: 100vw;
    height: 100vh;
    perspective: 200px;
    transform-style: preserve-3d;
}
.inner {
    width: 100%;
    height: 100%;
    margin-top: -20%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    transform-style: preserve-3d;
    transform: translateY(calc(-50% + 100px)) translateZ(0) rotateX(90deg);
    transform-origin: bottom center;
    animation: move 8s infinite linear;
}
.item {
    width: 250px;
    height: 60px;
    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 10px;
    color: white;
    background: darkslateblue;
    transform: rotateX(-90deg);
}
@keyframes move {
    100% {
        transform: translateY(calc(-50% + 100px)) 
                   translateZ(calc(100vh + 120px)) 
                   rotateX(90deg);
    }
}
</style>
</head>
<body>
<div class="wrapper">
    <div class="inner">
        <div class="item">(重拳)🧐</div>
        <div class="item">快走吧</div>
        <div class="item">洋芋片只要半价</div>
        <div class="item">那边的便利商店</div>
        <div class="item">要来不及了</div>
        <div class="item">站住</div>
        <div class="item">趁风停止之前</div>
        <div class="item">快走吧</div>
        <div class="item">带到镇上去了</div>
        <div class="item">这股风把一些坏东西</div>
        <div class="item">快走吧</div>
        <div class="item">在哭泣的样子</div>
        <div class="item">不过这风似乎···</div>
        <div class="item">甚是喧嚣呢</div>
        <div class="item">今天的风儿</div>
    </div>
</div>
</body>
</html>
```

[<font size = 5>源自</font>](https://juejin.cn/post/6844903654458146823#heading-5)

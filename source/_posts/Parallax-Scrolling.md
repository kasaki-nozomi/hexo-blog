---
title: 视差滚动
date: 2022-09-03 18:40:22
description: 视差滚动 Parallax Scrolling
categories:
- CSS
tags:
- CSS
---

***

**<font size = 6 color = dd00dd>视差滚动</font>**

***

<b>视差滚动 `Parallax Scrolling`</b> 指多层背景以不同的速度移动，形成立体的运动效果

实现方式

- `background-attachment`
- `transform: translate3D`
## Background-Attachment
<b>`background-attachment`：设置背景图像是否固定或者随着页面的其余部分滚动</b>

- <b>`scroll`</b> 默认，背景图片随<b>页面</b>的滚动而滚动
- <b><code style="color:#dd00dd">fixed</code> <font color = dd00dd>背景图片不随着页面的滚动而滚动</font></b>
- <b>`local`</b> 背景图片随着<b>元素内容</b>的滚动而滚动
- <b>`initial`</b> 设置该属性的默认值
- <b>`inherit`</b> 从父元素继承

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
      url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/m.png);
}
.img-four {
    background-image: 
      url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/b.png);
}
.image-six {
    background-image: 
      url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/u.png);
}
.image-six {
    background-image: 
      url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/k.png);
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
    <div>SEVEN</div>
    <div class="image-eight">EIGHT</div>
</body>
</html>
<!-- 代码中的图可能要梯子才能显示出来 -->
```

<div style="">
    <div style="
        height: 600px;
        line-height: 600px;
        font-size: 160px;
        text-align: center;
        color: white;
        background: black;
        background-attachment: fixed;
        background-size: cover;
        background-position: center center;
      ">
        A
    </div>
    <div style="
        background-image: url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/m.png);
        height: 600px;
        line-height: 600px;
        font-size: 160px;
        text-align: center;
        color: white;
        background-attachment: fixed;
        background-size: cover;
        background-position: 80px center;
      ">B</div>
    <div style="
        height: 600px;
        line-height: 600px;
        font-size: 160px;
        text-align: center;
        color: white;
        background: black;
        background-attachment: fixed;
        background-size: cover;
        background-position: center center;
      ">
        C
    </div>
    <div style="
        background-image: url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/b.png);
        height: 600px;
        line-height: 600px;
        font-size: 160px;
        text-align: center;
        color: white;
        background-attachment: fixed;
        background-size: cover;
        background-position: 80px center;
      ">
        D
    </div>
    <div style="
        height: 600px;
        line-height: 600px;
        font-size: 160px;
        text-align: center;
        color: white;
        background: black;
        background-attachment: fixed;
        background-position: center center;
      ">
        E
    </div>
    <div style="
        background-image: url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/u.png);
        height: 600px;
        line-height: 600px;
        font-size: 160px;
        text-align: center;
        color: white;
        background-attachment: fixed;
        background-size: cover;
        background-position: 80px center;
      ">
        F
    </div>
    <div style="
        height: 600px;
        line-height: 600px;
        font-size: 160px;
        text-align: center;
        color: white;
        background: black;
        background-attachment: fixed;
        background-position: center center;
      ">
        G
    </div>
    <div style="
        background-image: url(https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/k.png);
        height: 600px;
        line-height: 600px;
        font-size: 160px;
        text-align: center;
        color: white;
        background-attachment: fixed;
        background-size: cover;
        background-position: 80px center;
      ">
        H
    </div>
</div>




## Transform-style

<b><code style="color:#dd00dd">transform-style</code> 属性：</b>

- `flat`：所有子元素在 `2D` 平面呈现
- `preserve-3d`：所有子元素在 `3D` 空间中呈现

<b><code style="color:#dd00dd">perspective</code> 属性：（做一个透视）</b>

- <b>`translateZ` 的值为 `perspective` 的值</b>时，元素大小为屏幕大小
- `number`：元素距离视图的距离，以像素计（相当于人离屏幕 `?px` 的位置观看指定元素）
- `none`：默认值，与 `0` 相同，不设置透视

<b>`perspective-origin: x y` 属性：</b>

- <b>`x`: </b>`left | center | right | length | %` 定义该视图在 `x` 轴上的位置，默认`50%`
-  <b>`y`: </b>`top | center | bottom | length | %` 定义该视图在 `y` 轴上的位置，默认`50%`

<b>`transform-origin: x y z` 属性：</b>

- <b>`x`:</b> `left | center | right | length | %` 定义视图被置于 `X` 轴何处
- <b>`y`:</b> `top | center | bottom | length | %` 定义视图被置于 `Y` 轴何处
- <b>`z`:</b> `length` 定义视图被置于`Z`轴何处

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

  <div style="width: 100%; height: 70%; overflow: hidden">
    <div
      style="
        width: 100%;
        height: 70%;
        perspective: 200px;
        transform-style: preserve-3d;
      "
    >
      <div
        style="
          width: 100%;
          height: 70%;
          margin-top: -30%;
          display: flex;
          flex-direction: column;
          justify-content: center;
          align-items: center;
          transform-style: preserve-3d;
          transform: translateY(calc(-50% + 100px)) translateZ(0) rotateX(90deg);
          transform-origin: bottom center;
          animation: move 8s infinite linear;
        "
      >
        <style>
          @keyframes move {
            100% {
              transform: translateY(calc(-50% + 100px))
                translateZ(calc(100vh + 120px)) rotateX(90deg);
            }
          }
        </style>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          (重拳)🧐
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          快走吧
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          洋芋片只要半价
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          那边的便利商店
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          要来不及了
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          站住
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          趁风停止之前
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          快走吧
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          带到镇上去了
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          这股风把一些坏东西
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          快走吧
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          在哭泣的样子
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          不过这风似乎···
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          甚是喧嚣呢
        </div>
        <div
          style="
            width: 250px;
            height: 60px;
            display: flex;
            justify-content: center;
            align-items: center;
            border-radius: 10px;
            color: white;
            background: darkslateblue;
            transform: rotateX(-90deg);
          "
        >
          今天的风儿
        </div>
      </div>
    </div>
  </div>

[<font size = 5>源自</font>](https://juejin.cn/post/6844903654458146823#heading-5)

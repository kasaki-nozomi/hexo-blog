---
title: JS 与 CSS3 实现瀑布流
date: 2022-08-31 10:50:22
description: 分别通过 JS 的方式与纯 CSS3 样式实现瀑布流
categories:
- JavaScript
tags:
- JavaScript
- CSS
---

**<font color="#dd00dd" size=6>瀑布流</font>**

***

瀑布流的意思大概是：每列宽度相等而高度不等，且下一个容器需要放在当前高度最小的列的容器下，依次类推放置<br />可以通过`JS`实现或者`CSS`实现，`CSS`可以通过`float`浮动或者`CSS3`的样式来实现，`float`简单但是列数固定，不过不太好扩展
## JS 实现
```html
<html>
<head>
  <title>瀑布流</title>
</head>
<style>
* {
  margin: 0;
  padding: 0;
}
body {
  width: 100vw;
  height: 100vh;
  background-color: lightgray;
}
.container {
  width: 100vw;
  height: 100vh;
  overflow: auto;
  position: relative;
}
.item {
  width: 200px;
  height: auto;
  top: 0;
  left: 0;
  position: absolute;
  box-sizing: border-box;
  border: 2px solid black;
  border-radius: 15px;
  font-size: 40px;
  font-weight: bold;
  text-align: center;
  background: white;
}
</style>
<body>
  <div class="container">
    <div class="item" style="height: 250px">1</div>
    <div class="item" style="height: 150px">2</div>
    <div class="item" style="height: 200px">3</div>
    <div class="item" style="height: 250px">4</div>
    <div class="item" style="height: 150px">5</div>
    <div class="item" style="height: 250px">6</div>
    <div class="item" style="height: 300px">7</div>
    <div class="item" style="height: 250px">8</div>
    <div class="item" style="height: 250px">9</div>
    <div class="item" style="height: 150px">0</div>
  </div>
</body>
<script>
function waterFullLayout() {
  let pageWidth = document.body.clientWidth
  let itemWidth = document.getElementsByClassName('item')[0].offsetWidth
  let itemGap = 10
  let nums = Math.floor(pageWidth / (itemWidth + itemGap))
  let heightList = []
  let itemArray = document.getElementsByClassName('item')
  let len = itemArray.length
  for (let i = 0; i < len; i++) {
    if (i < nums) {
      heightList.push(itemArray[i].offsetHeight + itemGap)
      itemArray[i].style.top = itemGap + 'px'
      itemArray[i].style.left = (itemWidth + itemGap) * i 
							  + (pageWidth - (itemWidth + itemGap) * nums) / 2 
							  + 'px'
    } else {
      let minItem = { minHeight: heightList[0], minIndex: 0 }
      for (let j = 0; j < heightList.length; j++) {
        if (heightList[j] < minItem['minHeight']) {
          minItem['minHeight'] = heightList[j]
          minItem['minIndex'] = j
        }
      }
      itemArray[i].style.top = minItem['minHeight'] + itemGap + 'px'
      itemArray[i].style.left = (itemWidth + itemGap) * minItem['minIndex']
        					  + (pageWidth - (itemWidth + itemGap) * nums) / 2 
        					  + 'px'
      heightList[minItem['minIndex']] = parseFloat(itemArray[i].style.top) 
        							  + parseFloat(itemArray[i].offsetHeight)
    }
  }
}
function debounce(func, delay) {
  let timeout = null
  return function () {
    if (timeout) {
      clearTimeout(timeout)
    }
    timeout = setTimeout(() => {
      func.apply(this, arguments)
    }, delay)
  }
}
window.onload = function () {
  waterFullLayout()
}
window.onresize = debounce(waterFullLayout, 100)
</script>
</html>
```
**效果：**<br />![image.png](https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-01.png)

## CSS3 实现
```html
<html>
<head>
    <title>瀑布流</title>
</head>
<style>
.container {
    -webkit-column-width: 160px;
    -moz-column-width: 160px;
    -o-colum-width: 160px;
    -webkit-column-gap: 5px;
    -moz-column-gap: 5px;
    -o-column-gap: 5px;
}
div:not(.container) {
    width: 160px;
    margin: 2px 0;
    border: gray 1px solid;
    border-radius: 5px;
    display: inline-block;
    background: green;
}
</style>
<body>
<div class="container">
    <div style="height:260px"></div>
    <div style="height:060px"></div>
    <div style="height:120px"></div>
    <div style="height:145px"></div>
    <div style="height:100px"></div>
    <div style="height:145px"></div>
    <div style="height:160px"></div>
    <div style="height:100px"></div>
    <div style="height:130px"></div>
    <div style="height:140px"></div>
    <div style="height:105px"></div>
    <div style="height:070px"></div>
    <div style="height:145px"></div>
    <div style="height:150px"></div>
    <div style="height:065px"></div>
    <div style="height:230px"></div>
    <div style="height:140px"></div>
    <div style="height:085px"></div>
    <div style="height:090px"></div>
    <div style="height:145px"></div>
    <div style="height:090px"></div>
</div>
</body>
</html>
```
**效果：**<br />![image.png](https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-02.png)

---
title: 用 CSS 画一个聊天框
date: 2022-09-21 10:40:22
description: CSS 聊天框
categories:
- CSS
tags:
- CSS
---

## ONE

<div style="
  height: 60px;
  width: auto;
  position: relative;
  display: inline-block;
  font-size: 20px;
  line-height: 60px;
  text-align: center;
  color: aliceblue;
  padding: 0 25px;
  border-radius: 10px;
  background-color: blueviolet">
  你好 谢谢 小笼包 拜拜
  <div style="
    width: 0;
    height: 0;
    position: absolute;
    right: 20px;
    margin-left: 100px;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-top: 20px solid blueviolet;
    transform: skewX(45deg)">
  </div>
</div>

**代码实现：**

```css
<html>
<style>
  #chat {
    height: 60px;
    width: auto;
    display: inline-block;
    position: relative;
    font-size: 20px;
    line-height: 60px;
    text-align: center;
    color: aliceblue;
    padding: 0 25px;
    border-radius: 10px;
    background-color: blueviolet;
  }
  #chat::after {
    content: '';
    position: absolute;
    width: 0;
    height: 0;
    right: 20px;
    bottom: -20px;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-top: 20px solid blueviolet;
    transform: skewX(45deg);
  }
</style>
<div id="chat">
  你好 谢谢 小笼包 拜拜
</div>
</html>
```

## TWO

<div style="
  height: 60px;
  width: auto;
  display: inline-block;
  position: relative;
  font-size: 20px;
  line-height: 60px;
  text-align: center;
  color: aliceblue;
  padding: 0 25px;
  margin-top: 20px;
  border-radius: 10px;
  background-color: blueviolet">
  你好 谢谢 小笼包 拜拜
  <div style="
    width: 0;
    height: 0;
    top: -20px;
    right: 20px;
    display: inline-block;
    position: absolute;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-bottom: 20px solid blueviolet;
    transform: skewX(-45deg);">
</div>
</div>

```css
<html>
<style>
  #chat {
    height: 60px;
    width: auto;
    display: inline-block;
    position: relative;
    font-size: 20px;
    line-height: 60px;
    text-align: center;
    color: aliceblue;
    padding: 0 25px;
    margin-top: 20px;
    border-radius: 10px;
    background-color: blueviolet;
  }
  #chat::after {
    content: '';
    position: absolute;
    width: 0;
    height: 0;
    top: -20px;
    right: 20px;
    border-left: 10px solid transparent;
    border-right: 10px solid transparent;
    border-bottom: 20px solid blueviolet;
    transform: skewX(-45deg);
  }
</style>
<div id="chat">
  你好 谢谢 小笼包 拜拜
</div>
</html>
```

## THREE

<div style="
  height: 60px;
  width: auto;
  display: inline-block;
  position: relative;
  font-size: 20px;
  line-height: 60px;
  text-align: center;
  color: aliceblue;
  padding: 0 25px;
  border-radius: 10px;
  background-color: blueviolet">
  你好 谢谢 小笼包 拜拜
  <div style="
    width: 0;
    height: 0;
    right: -30px;
    top: 20px;
    display: inline-block;
    position: absolute;
    border-top: 10px solid transparent;
    border-bottom: 10px solid transparent;
    border-left: 30px solid blueviolet;">
</div>
</div>

**代码实现：**

```css
<html>
<style>
  #chat {
    height: 60px;
    width: auto;
    display: inline-block;
    position: relative;
    font-size: 20px;
    line-height: 60px;
    text-align: center;
    color: aliceblue;
    padding: 0 25px;
    border-radius: 10px;
    background-color: blueviolet;
  }
  #chat::after {
    content: '';
    position: absolute;
    width: 0;
    height: 0;
    right: -30px;
    bottom: 20px;
    border-top: 10px solid transparent;
    border-bottom: 10px solid transparent;
    border-left: 30px solid blueviolet;
  }
</style>
<div id="chat">
  你好 谢谢 小笼包 拜拜
</div>
</html>
```

## FOUR

<div style="
  height: 60px;
  width: auto;
  display: inline-block;
  position: relative;
  font-size: 20px;
  line-height: 60px;
  text-align: center;
  color: aliceblue;
  padding: 0 25px;
  margin-left: 30px;
  border-radius: 10px;
  background-color: blueviolet">
  你好 谢谢 小笼包 拜拜
  <div style="
    width: 0;
    height: 0;
    left: -30px;
    top: 20px;
    display: inline-block;
    position: absolute;
    border-top: 10px solid transparent;
    border-bottom: 10px solid transparent;
    border-right: 30px solid blueviolet;">
</div>
</div>

**代码实现：**

```CSS
<html>
<style>
  #chat {
    height: 60px;
    width: auto;
    display: inline-block;
    position: relative;
    font-size: 20px;
    line-height: 60px;
    text-align: center;
    color: aliceblue;
    padding: 0 25px;
    margin-left: 30px;
    border-radius: 10px;
    background-color: blueviolet;
  }
  #chat::after {
    content: '';
    position: absolute;
    width: 0;
    height: 0;
    left: -30px;
    bottom: 20px;
    border-top: 10px solid transparent;
    border-bottom: 10px solid transparent;
    border-right: 30px solid blueviolet;
  }
</style>
<div id="chat">
  你好 谢谢 小笼包 拜拜
</div>
</html>
```

## 附

> 一个带边框的三角形

<div style="
    width: 0px; 
    height: 0px;
    position: relative;
    border-left: 50px solid transparent; 
    border-right: 50px solid transparent; 
    border-bottom: 90px solid blueviolet; ">
    <div style="
        width: 0px;
        height: 0px;
        position: absolute;
        top: 12px;
        right: -40px;
        border-left: 40px solid transparent;
        border-right: 40px solid transparent;
        border-bottom: 72px solid white;">
    </div>
</div>

**代码实现：**

```css
<html>
  <div id="triangle"></div>
  <style>
  #triangle { 
      width: 0px; 
      height: 0px;
      position: relative;
      border-left: 50px solid transparent; 
      border-right: 50px solid transparent; 
      border-bottom: 90px solid blueviolet; 
  }
  #triangle::after {
      content: '';
      width: 0px; 
      height: 0px;
      position: absolute;
      top: 12px;
      right: -40px;
      border-left: 40px solid transparent; 
      border-right: 40px solid transparent; 
      border-bottom: 72px solid white; 
  }
</style>
</html>
```

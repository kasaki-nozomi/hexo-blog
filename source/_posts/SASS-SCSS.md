---
title: SASS SCSS
date: 2022-07-09 20:20:22
description: CSS 预处理器：SASS/SCSS
categories:
- CSS
tags:
- CSS
- SASS
- SCSS
---
***
**<font color=dd00dd size=6>SASS / SCSS</font>**

***
`Sass`是一款强化`CSS`的辅助工具

`SASS`在`CSS`基础上增加了：**变量**`variables`  **嵌套**`nested rules`  **混合**`mixins`  **导入**`inline imports` 等

## BEFORE
`Sass`基于`Ruby`语言开发而成，安装`Sass`前需要安装`Ruby`（`Mac`自带不需要安装）

`Sass`和`Compass`安装：（`Compass`是`Sass`的工具库）

`gem install sass`

`gem install compass`

## SASS 与 LESS
`Sass`与`Less`都是一种`CSS`预处理器

`Sass`基于`Ruby`语言，在安装`Sass`前需要安装`Ruby`，`Less`是基于`JS`

`Sass`在服务端处理，`Less`在客户端处理，**前者**解析**稍快**

`Sass`使用`$`，`Less`使用`@`，编写变量的方式

## SCSS 与 SASS
`SCSS`：`Sassy CSS`，`Sass`的一种格式，是`CSS3`语法基础的拓展，同时加入`Sass`的特色功能

`.scss`作为拓展名

`SASS`：`Sass`的另一种格式：缩进格式`Indented Sass`，使用缩进代替花括号，换行代替分号

`.sass`作为拓展名

`Sass3.0`版本之后为`SCSS`

`SCSS`与 `SASS`格式之间相互转换：（使用`sass-convert`）

`sass-convert style.sass style.scss`

`sass-convert style.scss style.sass`

## SASS 编译
``` bash
// 单文件转换命令
sass input.scss output.css

// 单文件监听命令
sass --watch input.scss:output.css

// 监听包含很多 sass 文件的目录
sass --watch app/sass:public/stylesheets
```
四种编译格式：`nested` `expanded` `compact` `compressed`

可使用`sass style.scss:style.css --style [format]`指定编译格式

## SCSS ON
## 变量
声明一个变量：`$basic-color: pink;`

可以用作`CSS`属性值的赋值都可以用作`Scss`的变量值：`$basic-border: 1px solid black;`

以逗号分割的多个属性值：`?`

重复声明变量只有最后的生效

与`CSS`属性不同，变量可以在`CSS`规则块定义之外存在，变量定义规则块内时只能在此块内使用

使用定义的变量：`div { borer: $basic-border; }` `div { borer: 1px solid $base-color; }`

中`or`下划线无所谓：`div { color: $basic_color; }` `=>` `div { color: pink; }`

## 嵌套
```css
#content {
    header {
        p { color: pink }
        a { margin-bottom: 10px }
    }
    footer { background-color: yellow }
}

/* compile */

#content header p { color: pink }
#content header a { margin: 10px }
#content footer { background-color: yellow }
```
```css
.container {
    h1, h2, h3 { margin-bottom: 10px }
}

/* compile */

.container h1, .container h2, .container h3 { margin: 10px }

header, footer {
    p { color: blue }
}

/* compile */

header p, footer p { color: blue }
```
```css
content {
    ~ content { color: yellow }
    > section { color: yellow }
    section > {
        one { color: yellow }
        two { color: yellow }
    }
    header + & { color: yellow }
}

/* compile */

content ~ content { color: yellow }
content > section { color: yellow }
content section > one { color: yellow }
content section > two { ccolor: yellow }
header + content { color: yellow }
```
```css
div {
    border: {							/* 有个冒号！ */
        width: 1px;
        style: solid;
        color: yellow;
    }
}

/* compile */

div {
    border-width: 1px;
    border-style: solid;
    border-color: yellow;
}
```
上述通过空格拼接得到原生样式

使用伪类（或其他情况）时使用`&`符号：`&`不在会被拼接，而是被父选择器直接替换

```css
article a {
    color: blue;
    &:hover { color: red }
}

/* compile */

article a { color: blue }
article a:hover { color: red }
```
## 导入
不同于`CSS`的执行到`@import`才去导入对应的样式文件，`Scss`的`@import`规则是在生成`CSS`文件时就把相关文件导入进来

命名规定：`Scss`局部文件的文件名以**下划线开头**，`Scss`不会在编译时单独编译这个文件输出`CSS`

导入时可省略扩展名`.scss` `.sass`：`@import "color"`

导入时可省略开头的下划线：导入`themes/_night.scss` `=>` `@import "themes/night"`

重复声明变量只有最后的生效，若导入了相同变量名的变量

可使用`!default`标签实现导入的文件中的变量覆盖原存在的变量：`$basic-color: red !default;`，导入的`$basic-color`会覆盖此值
**嵌套导入：**

```css
/* _blue-theme.scss */
header {
    background: blue;
    color: white;
}

/* own.css */
.blue-theme { @import "blue-theme" }

/* compile */

.blue-theme {
    header {
        background: blue;
        color: white;
    }
}
```
**原生**`CSS`**导入：**

下面三种情况下会生成原生的`CSS` `@import`：

- 被导入文件名以`.css`结尾
- 被导入文件名是`URL`地址
- 被导入文件名是`CSS`的`url()`值

**静默注释：**
```css
body {
    color: yellow; 	// 这种注释内容不会出现在生成的 css 文件中
    padding: 10px; 	/* 这种注释内容会出现在生成的 css 文件中 */
}
```
## 混合器
可以通过`Scss`的混合器实现大段样式的重用：`@mixin` `@include`
```css
@mixin jashin-style {
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
    border-radius: 10px;
}

div {
    background-color: green;
    border: 1px solid yellow;
    @include jashin-style;
}

/* compile */

div {
    background-color: green;
    border: 1px solid yellow;
    -moz-border-radius: 10px;
    -webkit-border-radius: 10px;
    border-radius: 10px;
}
```
```css
@mixin no-bullets {
    list-style: none;
    li {
        list-style-image: none;
        list-style-type: none;
        margin-left: 0px;
    }
}

ul.plain {
    color: blue;
    @include no-bullets;
}

/* compile */

ul.plain {
    color: blue;
    list-style: none;
}
ul.plain li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
}
```

**向混合器传参：**

参数**默认值**使用`$name: default-value`

```css
@mixin link-colors($normal, $hover: $normal, $visited: red) {
    color: $normal;
    &:hover { color: $hover; }
    &:visited { color: $visited; }
}

a {
    @include link-colors(blue, red, green);
    /* OR */
    @include link-colors($normal: blue, $visited: green, $hover: red);
}

/* compile */

a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```
## 继承
通过`@extend`实现继承
```css
.error {
    border: 1px solid red;
    background-color: yellow;
}
.serious-error {
    @extend .error;
    border-width: 5px;
}
```
上述的`.serious-error`将会继承样式表中任何位置处为`.error`定义的所有样式

以`class="serious-error"`修饰的元素最终展示效果就好像是`class="serious-error error"`

不仅继承`.error`自身样式，跟`.error`有关的组合选择器也会被`.serious-error`以相应形式继承

```css
/* .serious-error a */
.error a {
    color: red;
    font-weight: 100;
}
/* h1 .serious-error */
h1.error {
    font-size: 20px;
}
```
若一条样式规则继承了一个复杂的选择器，那么它只会继承这个复杂选择器命中的元素所应用的样式

比如`.serious-error` `@extend.important.error`

则`.important.error` `p.important.error`的样式会被`.serious-error`继承

但`.important`或`.error`下的样式不会被继承

又比如`.main .serious-error` `@extend .error`，则只有完全匹配`.main .serious-error`的元素才会继承`.error`的样式

**继承原理：**

若`.serious-error` `@extend .error`，则任何一处`.error`都用`.error .serious-error`选择器组进行替换

跟混合器相比，继承生成的`CSS`代码相对更少（体积小），继承仅仅是重复选择器，而不会重复属性

若是继承中了出现了相同的属性，则根据权重进行选择，权重一样则使用之后定义的属性

尽量不要在`CSS`规则中使用后代选择器（比如`.foo .bar`）去继承`CSS`规则

## VUE 中的使用
**创建项目时选择预处理器**

需要的依赖（会自动安装好）：

- `npm install sass-loader --save-dev`
- `npm install node-sass --sava-dev`

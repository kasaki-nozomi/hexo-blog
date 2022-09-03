---
title: DOM 操作与 CSS 选择器
date: 2022-07-11 22:20:22
description: 一些 DOM 的简单操作与 CSS 的选择器与伪类与伪元素
categories:
- CSS
tags:
- DOM
- HTML
- CSS
---

## DOM

`DOM`（`Document Object Model`——文档对象模型）

##  DOCUMENT
***
- `document.getElementById(id)`        返回匹配特定`ID`的元素        
- `document.getElementByClassName(class)`        返回包含所有指定类名的子元素的类数组对象
- `document.getElementByTagName(tag)`        返回包括所有给定标签名称元素的`HTML`集合
- `document.querySelector(selectors)`        返回与指定选择器匹配的第一个`Element`对象
- `document.querySelectorAll(selectors)`        返回与指定的选择器匹配的文档中的元素列表
***
```javascript
// 获取文档中第一个 <span> 元素
document.querySelector("span")

// 获取文档中 class="demo" 的第一个元素
document.querySelector(".demo")

// 获取文档中 class="demo" 的第一个 <span> 元素
document.querySelector("span.demo")

// 获取文档中有 target 属性的第一个 <a> 元素
document.querySelector("a[target]")

// 设置文档中第一个 <span> 的背景颜色为粉红色
document.querySelector("span").style.backgroundColor = "pink"

// class 为 user-panel main 的 div 内包含一个 name 属性为 login 的 input
document.querySelector("div.user-panel.main input[name='login']")	
```
***
- `node.firstElementChild`        返回对象的**第一**个子元素，没有则为`null`
- `node.lastElementChild`        返回对象的**最后**一子元素，没有则为`null`
- `node.parentNode`        返回`node`的父节点
- `node.children()`        返回`node`的子节点
- `document.createElement(tag_name)`        创建由标签名称`tag_name`指定的`HTML`元素
***
```javascript
function add_element () {
    const new_div = document.createElement("div")
    const new_content = document.createTextNode("Hi there and greetings!")
    new_div.appendChild(new_content)

    const current_div = document.getElementById("id")
    document.body.insertBefore(new_div, current_div)
}
```
***
- `node.cloneNode(deep)`        克隆节点并返回，`deep`为`true`深度克隆
- `node.insertBefore(new, refer)`        在参考节点之前插入拥有指定父节点的子节点并返回
- `parent.removeChild(child)`        删除一个子节点并返回删除的节点
- `node.append(node)`        在最后一个子节点之后插入一组节点
- `node.prepend(node)`        在第一个子节点之前插入一组节点
- `node.appendChild(node)`        将节点加到指定节点的子节点的末尾并返回
   - 若被插入的节点**已存在于文档树**中，`appendChild()`只会将其从原先位置**移动到新位置**
   - 若被插入的节点**已拥有父节点**，在被传递给此方法后，先**移除**，再**插入**到新的位置
   - 若要**保留**已在文档中的节点，可先使用`node.cloneNode()`创建一个副本
- `node.append(node|str)`与`node.appendChild(node)`差异
   - `node.append()`**可追加**`DOMString`对象，而`node.appendChild()`只接受`node`对象
   - `node.append()`**无返回值**，`Node.appendChild()`返回追加的`node`对象。
   - `node.append()`**可追加多个**节点和字符串，`node.appendChild()`只可追加一个节点
## CSS 选择器
***
- `* {}`        通配选择器（所有元素）
- `p {}`        标签选择器
- `.class {}`        类选择器
   - `a.value {}`        匹配`class`属性包含`value`的所有`<a>`元素
- `#id {}`        `ID`选择器（区分大小写）（`ID`不能重复）
- `div, span {}`        选择器列表
- `div span {}`        后代选择器
   - `span a {}`        对`<span>`中的`<a>`元素应用样式（可以隔很多层）
- `div>span {}`        直接子代选择器
   - `span > a {}`        选择作为`<span>`**直接子元素**的所有`<a>`元素
   - `table.company td > p {}`        选择`class`为`table`的`td`下的`<p>`元素
- `A~B {}`        一般兄弟选择器，不需要紧邻，在`A`后的`B`就行（兄弟元素）
- `A+B {}`        紧邻兄弟选择器，紧接在`A`后出现的`B`（兄弟元素）
- `col || td`        列选择器
- `[attr[=value]] {}`        属性选择器（属性与属性值必须完全匹配）
   - `*[href] {}`        包含`href`属性的所有元素
   - `a[href] {}`        包含`href`属性的所有`<a>`元素	
   - `a[href][title] {}`        包含`href``+``title`属性的`<a>`元素
   - `a[href="..."] {}`        包含指定`href`属性值的所有`<a>`元素
   - `a[class="one two"] {}`        某个元素`class="one two"`则必须两个属性都写上
   - `a[class~="one"] {}`        根据属性值中的词列表的**某个词**进行选择：`~`
   - `a[value^="def"] {}`        选择`value`属性值以`def`**开头**的所有元素
   - `a[value$="def"] {}`        选择`value`属性值以`def`**结尾**的所有元素
   - `a[value*="def"] {}`        选择`value`属性值中包含**子串**`def`的所有元素
   - `a[value|="def"] {}`        选择`value`属性等于`def`或以`def-`开头的所有元素

**选择器优先级算法：**
- 每个规则对应一个初始`4`位数`0、0、0、0`
- 若是行内选择符，则加`1、0、0、0`
- 若是`ID`选择符，则加`0、1、0、0`
- 若是类选择符/伪类选择符，则加`0、0、1、0`
- 若是元素选择符，则加`0、0、0、1`

**优先级（递减）：**
- 首先`ID`选择器
- 类选择器        **属性选择器**（例`[type="radio"]`）        **伪类**（例`:hover`）
- 标签选择器        **伪元素**（例`::before`）
- 通配选择器

**其他：**
- 带有 **`!important`** 标记的样式属性的优先级最高
- **内联样式**`>`**内部样式**`>`**外部样式**`>`**浏览器用户自定义样式**`>`**浏览器默认样式**
- **内部样式（**写到`head`中的`style`标签中的样式**）**
- **通配选择符**（`*`）**关系选择符**（`+``>``~``''``||`）**否定伪类**（`:not()`）对优先级没有影响
## CSS 伪类
伪类用于定义元素的**特殊状态**
- `a:hover {}`        选择鼠标悬停的`<a>`元素                
- `a:active {} `        选择活动的`<a>`元素（鼠标按下时）
- `a:visited {}`        选择所有被访问过的`<a>`元素
- `input:focus {}`         选择获得焦点的`<input>`元素
- `:not(p) {}`        选择每个非`<p>`元素的元素
- `p:first-child {}`        选择作为其父的首个子元素的每个`<p>`元素
- `p:first-of-type {}`        选择作为其父的首个`<p>`元素的每个`<p>`元素
- `p:last-child {}`        选择作为其父的最后一个子元素的每个`<p>`元素
- `p:last-of-type {}`        选择作为其父的最后一个`<p>`元素的每个`<p>`元素
- `p:nth-child(n) {}`        选择作为其父的第`n`个子元素的每个`<p>`元素
- `p:nth-last-child(n) {}`        选择作为其父的倒数第`n`个子元素的每个`<p>`元素
- `p:only-child {}`        选择作为其父的唯一子元素的每个`<p>`元素
- `p:nth-of-type(n) {}`        选择作为其父的第`n`个`<p>`元素的每个`<p>`元素
- `p:nth-last-of-type(n) {}`        选择作为其父的倒数第`n`个`<p>`元素的每个`<p>`元素
## CSS 伪元素
伪元素用于设置**元素指定部分**的样式
- `p::after {}`        在每个`<p>`元素之后插入内容
- `p::before {}`        在每个`<p>`元素之前插入内容
- `p::first-letter {}`        选择每个`<p>`元素的首字母
- `p::first-line {}`        选择每个`<p>`元素的首行
- `p::selection {}`        选择用户选择的元素部分（比如：选中的文本）

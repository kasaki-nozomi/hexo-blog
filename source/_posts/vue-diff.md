---
title: Vue Diff 算法
date: 2022-09-04 13:41:22
description: Virtual Dom 与 Vue Diff 算法与 Key 值的作用
categories:
- VUE
tags:
- VUE
---

***

**<font size = 6 color = dd00dd>Virtual Dom 与 Vue Diff 与 Key</font>**

***

## Virtual Dom

虚拟 `DOM` 是将真实的 `DOM` 的数据抽取出来，以对象的形式模拟树形结构，当状态改变时，构造新的虚拟数，新树旧树比较，把差异更新到真正的 `DOM` 树上，来更新视图

虚拟 `DOM` 的一个优势在于抽象了渲染过程，实现了<b>跨平台</b>的能力，而不仅仅局限于浏览器的 `DOM`，可以是安卓和 `IOS` 的原生组件，可以是小程序，也可以是各种 `GUI`

在 `JS` 对象中，虚拟 `DOM` 表现为一个 `Object` 对象，并且包含<b>标签名 `tag`、子元素对象 `children`</b> 等属性

创建 `VNode` 对象的话，可以使用 Vue 提供的 <b>`h` 函数</b>，其实最终都是调用 <b>`createElement()` 函数</b>创建的

<b>`Vnode` 类：</b>

```javascript
class VNode {
  tag?: string
  data: VNodeData | undefined
  children?: Array<VNode> | null
  text?: string
  elm: Node | undefined
  ns?: string
  context?: Component // rendered in this component's scope
  key: string | number | undefined
  componentOptions?: VNodeComponentOptions
  componentInstance?: Component // component instance
  parent: VNode | undefined | null // component placeholder node

  // strictly internal
  raw: boolean // contains raw HTML? (server only)
  isStatic: boolean // hoisted static node
  isRootInsert: boolean // necessary for enter transition check
  isComment: boolean // empty comment placeholder?
  isCloned: boolean // is a cloned node?
  isOnce: boolean // is a v-once node?
  asyncFactory?: Function // async component factory function
  asyncMeta: Object | void
  isAsyncPlaceholder: boolean
  ssrContext?: Object | void
  fnContext: Component | void // real context vm for functional nodes
  fnOptions?: ComponentOptions | null // for SSR caching
  devtoolsMeta?: Object | null // used to store functional render context for devtools
  fnScopeId?: string | null // functional scope id support
  // ......
}
```

<b>`h` 函数：</b>

```javascript
export function h(type: any, props?: any, children?: any) {
  if (!currentInstance) {
    __DEV__ &&
      warn(
        `globally imported h() can only be invoked when there is an active ` +
        `component instance, e.g. synchronously in a component's render or setup function.`
      )
  }
  return createElement(currentInstance!, type, props, children, 2, true)
}
```

## Vue Diff 算法

计算 `Virtual DOM` 树的改变部分（比较的是新旧虚拟 `DOM`）

<b>算法作用：</b>对比新旧 `Vnode` 过程中，找到与新 `Vnode` 对应的老 `Vnode`，复用真实的 `DOM` 节点，避免不必要的性能开销

<b>算法原理：</b>根据真实 `DOM` 生成一颗 `Virtual DOM`，当 `Virtual DOM` 某个节点 `oldVnode` 改变后会生成一个新的 `Vnode`，然后 `Vnode` 和 `oldVnode` 比较，发现有不一样的地方就<b>直接去真实的 <code>DOM</code> 上修改</b>，然后使 `oldVnode` 更新为`Vnode`，具体过程调用 `Patch` 方法，比较新旧节点，一边比较一边给真实 `DOM` 打补丁进行替换

<b>更具体：</b>

数据发生改变时，`set` 方法调用 `Dep.notify` 通知订阅者 `Watcher`，订阅者调用 <code style="color:#dd00dd"><b>patch</b></code> 方法为真实 `DOM` 打补丁

<b>同级比较，再比较子节点：</b>

在 <code style="color:#dd00dd"><b>patch</b></code> 方法中通过 <code style="color:#dd00dd"><b>sameVnode</b></code> 方法判断节点类型与 **`Key`** 值是否相同，如果节点类型与 **`Key`** 值不同，直接删掉原节点，再创建并插入新节点，不会再比较这个节点以后的子节点，若相同，则通过 <code style="color:#dd00dd"><b>patchVnode</b></code> 比较两个节点的内容与子节点

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-11.png" />

<b>递归比较子节点：</b>

若 <code style="color:#dd00dd"><b>patchVnode</b></code> 比较的两个节点都有子节点并且不完全一致，则调用 <code style="color:#dd00dd"><b>updateChildren</b></code>算法递归比较子节点

算法 <code style="color:#dd00dd"><b>updateChildren</b></code> 采用双端比较，分别从新旧 `Vnode` 的两端开始：
- 比较 `oldVNode` 与 `newVNode` 节点的 `startIndex` 与 `startIndex` 
- 比较 `oldVNode` 与 `newVNode` 节点的 `endIndex` 与 `endIndex`
- 比较 `oldVNode` 与 `newVNode` 节点的 `startIndex` 与 `endIndex` 
- 比较 `oldVNode` 与 `newVNode` 节点的 `endIndex` 与 `startIndex` 
- 若上面四种比较有一项相同则 <code style="color:#dd00dd"><b>patchVnode</b></code> ，同时移动 `oldVNode` 与 `newVNode` 的开始结束索引
- 若上面四种比较都不通过，则说明没有相同的节点可以复用，则会做下面两件事（看情况）：
	- 通过 `key` 值，找到与 `newNode` 的 `startIndex` 一致的旧的 `VNode` 节点，再进行 <code style="color:#dd00dd"><b>patchVnode</b></code>
	- 或者创建新的 `DOM` 节点放到当前 `newNode` 的 `startIndex` 位

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-08.png" />

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-09.png" />

新旧子节点只有顺序不同的时候，最佳操作通过移动元素位置来达到更新目的，通过 `Key` 值在新旧子的节点中保存映射关系，找到可复用的节点，`Key` 是唯一标识
复用判断：无 `Key` 值判断元素类型，有 `Key` 值判断元素类型 `+` `Key` 值

<font color=ddoodd size=5><b>VUE3 中</b></font>
<b>在 `Vue3` 中</b>，`Diff` 算法相比 `Vue2`，<b>为会发生变化的地方</b>添加一个 <b>`flag` 标记（静态标记）</b>

<b>关于静态标记：</b>

```typescript
export const enum PatchFlags {
      TEXT = 1,// 动态的文本节点
      CLASS = 1 << 1,  // 2 动态的 class
      STYLE = 1 << 2,  // 4 动态的 style
      PROPS = 1 << 3,  // 8 动态属性，不包括类名和样式
      FULL_PROPS = 1 << 4,  // 16 动态 key，当 key 变化时需要完整的 diff 算法做比较
      HYDRATE_EVENTS = 1 << 5,  // 32 表示带有事件监听器的节点
      STABLE_FRAGMENT = 1 << 6,   // 64 一个不会改变子节点顺序的 Fragment
      KEYED_FRAGMENT = 1 << 7, // 128 带有 key 属性的 Fragment
      UNKEYED_FRAGMENT = 1 << 8, // 256 子节点没有 key 的 Fragment
      NEED_PATCH = 1 << 9,   // 512
      DYNAMIC_SLOTS = 1 << 10,  // 动态 solt
      HOISTED = -1,  // 特殊标志是负整数表示永远不会用作 diff
      BAIL = -2 // 一个特殊的标志，指代差异算法
}
```
<b>例如：</b>`ID` 为 `two` 的 `<p>` 标签使用了插值表达式，则为其加一个 <b>`flag=1`</b> 标记，表示是可能发生变化的标签，而 `ID` 为 `one` 的 `<p>` 标签使用的是不会变动的值

对于不参与更新的元素，`Vue3` 会为其做<b>静态提升</b>，只会被创建一次，在渲染时直接复用
静态内容被放置在 <b>`render` 函数外</b>，每次渲染的时候只要取来其即可

```html
<div>
    <p id="one"> Hello Jashin </p>
    <p id="two">{{ jashin }}</p>
</div>
```
## KEY
节点中 `Key` 是每个 `vnode` 的唯一 `id`，也是 `diff` 的优化策略，可根据 `key`，更准确、 更快的找到对应的 `vnode` 节点

在 `v-for` 循环中 `key` 的作用是：通过判断 `newVnode` 和 `OldVnode` 的 `key` 与节点类型是否相等，从而复用与新节点对应的老节点，节约性能的开销

在`Vue` 里的 `Diff` 算法判断带 `Key` 值 `Vnode` 元素：
```typescript
// 检测两个节点是否相同
function sameVnode(a, b) {
  return (
    a.key === b.key &&
    a.asyncFactory === b.asyncFactory &&
    ((a.tag === b.tag &&
      a.isComment === b.isComment &&
      isDef(a.data) === isDef(b.data) &&
      sameInputType(a, b)) ||
      (isTrue(a.isAsyncPlaceholder) && isUndef(b.asyncFactory.error)))
  )
}
```

<b>不使用 `KEY` 时</b>：会发生 <b>`3` 次更新，`1` 次插入</b>操作（下图）

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-10.png" />

<b>使用 `KEY` 时：</b>只会发生 <b>`0` 次更新，`1` 次插入</b>操作（<b>`C` `D` `E`</b> 均会得到复用)

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-12.png" />

<b>注：</b>不要使用 `index` 作 `Key` 值，`index` 作 `key` 时和没有使用 `key` 值时，`Diff` 算法的流程没有变化，`index` 对于某一项不固定，比如在 `v-for` 数组中插入一项元素，此元素会占据之前的 `index`，判断时会与之前的此位置对应的 `DOM` 元素匹配，会发现 `Key` 值与元素类型相同，只是内容不同，更新内容，与未使用 `Key` 值时的匹配流程相同

<b>注：</b>更不要使用 `index` 拼接其他值作为 `key` 值，会导致一些情况下啥也匹配不到，全都无法复用

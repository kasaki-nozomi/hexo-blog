---
title: 图相关算法 JS 实现
date: 2022-09-10 11:20:22
description: 用 JS 实现图的相关算法（但是可能会有些大问题）
categories:
-  JavaScript
tags:
-  JavaScript
-  数据结构
---

**<font color=dd00dd size=5>用 JS 实现图的相关算法（但是可能会有些大问题）</font>**

## 深度优先遍历 DFS

<img width=600 src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-13.png">

```javascript
const graph = {
    0: [1, 4],
    1: [2, 4],
    2: [2, 3],
    3: [],
    4: [3],
}
const depthTraversal = function (graph, start) {
    const visited = new Set()
    const dfsResult = []
    const graphDfs = (n) => {
        dfsResult.push(n)
        visited.add(n)
        graph[n].forEach((c) => {
            if (!visited.has(c)) {
                graphDfs(c)
            }
        })
    }
    graphDfs(start)
    return dfsResult
}
```
## 广度优先遍历 BFS

<img width=600 src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-13.png">

```javascript
const graph = {
    0: [1, 4],
    1: [2, 4],
    2: [2, 3],
    3: [],
    4: [3],
}
const breadthTraversal = (graph, start) => {
    const visited = new Set()
    const queue = [start]
    const bfsResult = []
    visited.add(start)
    while (queue.length) {
        const n = queue.shift()
        bfsResult.push(n)
        graph[n].forEach((c) => {
            if (!visited.has(c)) {
                queue.push(c)
                visited.add(c)
            }
        })
    }
    return bfsResult
}
```
## 最短路径 Dijkstra

<img width=600 src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-14.png">

```javascript
const graph = [
    [0, 2, 4, 0, 0, 0],
    [0, 0, 2, 4, 2, 0],
    [0, 0, 0, 0, 3, 0],
    [0, 3, 0, 0, 0, 2],
    [0, 2, 0, 3, 0, 2],
    [0, 0, 0, 5, 0, 0],
]

const Dijkstra = function (graph, start = 0) {
    const len = graph.length
    const dist = new Array(len).fill(Infinity)
    const visited = new Array(len).fill(false)
    dist[start] = 0
    for (let i = 0; i < len; i++) {
        visited[start] = true
        for (let j = 0; j < len; j++) {
            if (graph[start][j] != 0 && graph[start][j] + dist[start] < dist[j]) {
                dist[j] = graph[start][j] + dist[start]
            }
        }
        let minIndex = null
        let min = Infinity
        for (let k = 0; k < len; k++) {
            if (!visited[k] && dist[k] < min) {
                min = dist[k]
                minIndex = k
            }
        }
        start = minIndex
    }
    return dist
}
```
**保存节点：**
```javascript
const graph = [
    [0, 2, 4, 0, 0, 0],
    [0, 0, 2, 4, 2, 0],
    [0, 0, 0, 0, 3, 0],
    [0, 3, 0, 0, 0, 2],
    [0, 2, 0, 3, 0, 2],
    [0, 0, 0, 5, 0, 0],
]
function Node(val, pre) {
    this.val = val
    this.pre = pre || null
}
function Dijkstra(graph, start = 0) {
    const len = graph.length
    const dist = new Array(len).fill(0).map(() => new Node(Infinity))
    const visited = new Array(len).fill(false)
    dist[start].val = 0
    while (visited.some(item => !item)) {
        visited[start] = true
        for (let j = 0; j < len; j++) {
            if (graph[start][j] != 0 && graph[start][j] + dist[start].val < dist[j].val) {
                dist[j].val = graph[start][j] + dist[start].val
                dist[j].pre = start
            }
        }
        let minIndex = null
        let min = Infinity
        for (let k = 0; k < len; k++) {
            if (!visited[k] && dist[k].val < min) {
                min = dist[k].val
                minIndex = k
            }
        }
        start = minIndex
    }
    return dist
}
```
## 最短路径 Floyd

<img width=600 src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-14.png">

```javascript
const graph = [
    [0, 2, 4, 0, 0, 0],
    [0, 0, 2, 4, 2, 0],
    [0, 0, 0, 0, 3, 0],
    [0, 3, 0, 0, 0, 2],
    [0, 2, 0, 3, 0, 2],
    [0, 0, 0, 5, 0, 0],
]
const floydWarshall = function (graph) {
    const dist = []
    const len = graph.length
    for (let i = 0; i < len; i++) {
        dist[i] = []
        for (let j = 0; j < len; j++) {
            dist[i][j] = graph[i][j]
        }
    }
    for (let k = 0; k < len; k++) {
        for (let i = 0; i < len; i++) {
            for (let j = 0; j < len; j++) {
                if (dist[i][k] != 0 && dist[k][j] != 0 && i != j) {
                    if (dist[i][j] === 0 || dist[i][k] + dist[k][j] < dist[i][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j]
                    }
                }
            }
        }
    }
    return dist
}
```
## 最小生成树 Prim

<img width=600 src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-15.png">

```javascript
const graph = [
    [0, 2, 1, 0, 0, 0],
    [2, 0, 2, 4, 2, 0],
    [1, 2, 0, 0, 3, 0],
    [0, 4, 0, 0, 3, 2],
    [0, 2, 3, 3, 0, 2],
    [0, 0, 0, 2, 2, 0],
]
const MSTPrim = function (graph, start = 0) {
    const len = graph.length
    const parent = new Array(len).fill(-1)
    const key = new Array(len).fill(Infinity)
    const visited = new Array(len).fill(false)
    key[start] = 0
    for (let i = 0; i < len - 1; i++) {
        let min = Infinity
        let minIndex = null
        for (let j = 0; j < len; j++) {
            if (!visited[j] && key[j] < min) {
                min = key[j]
                minIndex = j
            }
        }
        const index = minIndex
        visited[index] = true
        for (let k = 0; k < len; k++) {
            if (graph[index][k] && !visited[k] && graph[index][k] < key[k]) {
                parent[k] = index
                key[k] = graph[index][k]
            }
        }
    }
    const route = {}
    for(let i = 0; i < len; i++) {
        if(i === start) continue
        route[`${parent[i]}-${i}`] = key[i]
    }
    return route
}
```
## 最小生成树 Kruskal

<img width=600 src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-15.png">

```javascript
const graph = [
    [0, 2, 1, 0, 0, 0],
    [2, 0, 2, 4, 2, 0],
    [1, 2, 0, 0, 3, 0],
    [0, 4, 0, 0, 3, 5],
    [0, 2, 3, 3, 0, 2],
    [0, 0, 0, 5, 2, 0],
]
const find = (index, parent) => {
    while (parent[index] != null) {
        index = parent[index]
    }
    return index
}
const union = (u, v, parent) => {
    if (u !== v) {
        parent[v] = u
        return true
    }
    return false
}
const MSTKruskal = function (graph) {
    const len = graph.length
    const route = {}
    const parent = []
    let count = 0
    const copy = JSON.parse(JSON.stringify(graph))
    while (count < len - 1) {
        let a, b
        let min = Number.MAX_SAFE_INTEGER
        for (let i = 0; i < len; i++) {
            for (let j = 0; j < len; j++) {
                if (copy[i][j] < min && copy[i][j] != 0 && i != j) {
                    min = copy[i][j]
                    a = i
                    b = j
                }
            }
        }
        if (union(find(a, parent), find(b, parent), parent)) {
            route[`${a}-${b}`] = copy[a][b]
            count++
        }
        copy[a][b] = copy[b][a] = Number.MAX_SAFE_INTEGER
    }
    return route
}
```

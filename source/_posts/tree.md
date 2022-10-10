---
title: 树相关算法 JS 实现
date: 2022-09-19 14:20:22
description: 用 JS 实现树的相关算法
categories:
-  JavaScript
tags:
-  JavaScript
-  数据结构
---

## 二叉树前中后序遍历
```javascript
// 前序
const preorderTraversal = function (root) {
    const result = []
    const preOrder = (node) => {
        if (!node) {
            return
        }
        result.push(node.val)
        preOrder(node.left)
        preOrder(node.right)
    }
    preOrder(root)
    return result
}

// 中序
const inorderTraversal = function (root) {
    const result = []
    const inOrder = (node) => {
        if (!node) {
            return
        }
        inOrder(node.left)
        result.push(node.val)
        inOrder(node.right)
    }
    inOrder(root)
    return result
}

// 后序
const postorderTraversal = function (root) {
    const result = []
    const postOrder = (node) => {
        if (!node) {
            return
        }
        postOrder(node.left)
        postOrder(node.right)
      	result.push(node.val)
    }
    postOrder(root)
    return result
}
```
## 二叉树层序遍历
```javascript
const levelOrder = function (root) {
    const result = []
    if (!root) {
        return result
    }
    const queue = []
    queue.push(root)
    while (queue.length) {
        let len = queue.length
        while (len) {
            let node = queue.shift()
            result.push(node.val)
            if (node.left) queue.push(node.left)
            if (node.right) queue.push(node.right)
            len--
        }
    }
    return result
}
```
## 二叉树深度
```javascript
const maxDepth = function (root) {
    if(!root) {
        return 0
    }
    let left = maxDepth(root.left)
    let right = maxDepth(root.right)
    return 1 + Math.max(left, right)
}
```
## 前序＋中序构造二叉树
```javascript
const buildTree = function (preorder, inorder) {
    const len = preorder.length
    if (len === 0) {
        return null
    }
    const root = new TreeNode(preorder[0])
    const pos = inorder.indexOf(preorder[0])
    root.left = buildTree(preorder.slice(1, pos + 1), inorder.slice(0, pos))
    root.right = buildTree(preorder.slice(pos + 1, len), inorder.slice(pos + 1, len))
    return root
}
```
## 中序＋后序构造二叉树
```javascript
const buildTree = function (inorder, postorder) {
    const len = inorder.length
    if (len === 0) {
        return null
    }
    const root = new TreeNode(postorder[len - 1])
    const pos = inorder.indexOf(postorder[len - 1])
    root.left = buildTree(inorder.slice(0, pos), postorder.slice(0, pos))
    root.right = buildTree(inorder.slice(pos + 1, len), postorder.slice(pos, len - 1))
    return root
}
```
## 翻转二叉树
```javascript
const invertTree = function (root) {
    if (!root) {
        return null
    }
    [root.left, root.right] = [root.right, root.left]
    invertTree(root.left)
    invertTree(root.right)
    return root
}
```
## 二叉搜索树最近公共祖先
```javascript
const lowestCommonAncestor = function (root, p, q) {
    if (!root || root === p || root === q) {
        return root
    }
    if (p.val < root.val && q.val < root.val) {
        return lowestCommonAncestor(root.left, p, q)
    } else if (q.val > root.val && p.val > root.val) {
        return lowestCommonAncestor(root.right, p, q)
    } else {
        return root
    }
}
```
## 二叉树最近公共祖先
```javascript
const lowestCommonAncestor = function (root, p, q) {
    if (!root || root === p || root === q) return root
    const left = lowestCommonAncestor(root.left, p, q)
    const right = lowestCommonAncestor(root.right, p, q)
    if (!left) return right
    if (!right) return left
    return root
}

const lowestCommonAncestor = function (root, p, q) {
  let result
  const dfs = (node) => {
    if (!node) {
      return false
    }
    const lson = dfs(node.left)
    const rson = dfs(node.right)
    if ((lson && rson) || ((node.val == p.val || node.val == q.val) && (lson || rson))) {
      result = node
    }
    return lson || rson || node.val == p.val || node.val == q.val
  }
  dfs(root)
  return result
}
```
## 判断平衡二叉树
```javascript
const isBalanced = function (root) {
    const dfs = (node) => {
        if (!node) return 0
        const left = dfs(node.left)
        if (left === -1) return -1
        const right = dfs(node.right)
        if (right === -1) return -1
        return Math.abs(left - right) <= 1 ? Math.max(left, right) + 1 : -1
    }
    return dfs(root) !== -1
}
```


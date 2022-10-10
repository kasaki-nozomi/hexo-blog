---
title: 链表相关算法 JS 实现
date: 2022-09-19 14:40:22
description: 用 JS 实现链表相关算法
categories:
-  JavaScript
tags:
-  JavaScript
-  数据结构
---

## 反转链表
```javascript
const reverseList = function (head) {
    if (head == null || head.next == null) {
        return head
    }
    const newHead = reverseList(head.next)
    head.next.next = head
    head.next = null
    return newHead
}

const reverseList = (head) => {
    let prev = null
    let curr = head
    while (curr !== null) {
        let nextTemp = curr.next
        curr.next = prev
        prev = curr
        curr = nextTemp
    }
    return prev
}
```
## 合并两个有序链表
```javascript
const mergeTwoLists = function (l1, l2) {
    var list = new ListNode(-1)
    var result = list
    while (l1 !== null && l2 !== null) {
        if (l1.val <= l2.val) {
            list.next = l1
            l1 = l1.next
        }
        else {
            list.next = l2
            l2 = l2.next
        }
        list = list.next
    }
    list.next = (l1 == null) ? l2 : l1
    return result.next
}
```
## 合并 K 个有序链表
> **归并**

```javascript
const mergeKLists = function (lists) {
    if (lists.length === 0) {
        return null
    }
    function mergeList(start, end) {
        if (start === end) {
            return lists[start]
        }
        const mid = Math.floor((start + end) / 2)
        const leftList = mergeList(start, mid)
        const rightList = mergeList(mid + 1, end)
        return merge(leftList, rightList)
    }
    function merge(left, right) {
        const head = new ListNode(0)
        let node = head
        while (left && right) {
            if (left.val <= right.val) {
                node.next = left
                left = left.next
            } else {
                node.next = right
                right = right.next
            }
            node = node.next
        }
        node.next = right ? right : left
        return head.next
    }
    return mergeList(0, lists.length)
}
```
## 判断是否有环
```javascript
const hasCycle = function (head) {
    let fast = slow = head
    while (fast && fast.next) {
        fast = fast.next.next
        slow = slow.next
        if (fast == slow) {
            return true
        }
    }
    return false
}

// 找到环的入口
const detectCycle = function (head) {
    let a = head
    let b = head
    while (a && b && b.next) {
        a = a.next
        b = b.next.next
        if (a == b) {
            let c = head
            while (true) {
                if (a == c) {
                    return c
                } else {
                    a = a.next
                    c = c.next
                }
            }
        }
    }
    return null
}
```
## 倒序输出
```javascript
const reversePrint = function(head) {
    const result = []
    const reverse = (node) => {
        if(node) {
            reverse(node.next)
            result.push(node.val)
        }
    }
    reverse(head)
    return result
}
```
## 首个公共节点
![image.png](https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-16.png)
```javascript
const getIntersectionNode = function(headA, headB) {
    const visited = new Set()
    let temp = headA
    while (temp !== null) {
        visited.add(temp)
        temp = temp.next
    }
    temp = headB
    while (temp !== null) {
        if (visited.has(temp)) {
            return temp
        }
        temp = temp.next
    }
    return null
}

const getIntersectionNode = function (headA, headB) {
    if (headA === null || headB === null) {
        return null
    }
    let nodeA = headA
    let nodeB = headB
    while (nodeA !== nodeB) {
        nodeA = nodeA === null ? headB : nodeA.next
        nodeB = nodeB === null ? headA : nodeB.next
    }
    return nodeA
}
```
## 判断回文链表
```javascript
const isPalindrome = function (head) {
    if (head == null) return true

    const reverseList = (head) => {
        let prev = null
        let curr = head
        while (curr !== null) {
            let nextTemp = curr.next
            curr.next = prev
            prev = curr
            curr = nextTemp
        }
        return prev
    }
    const endOfFirstHalf = (head) => {
        let fast = head
        let slow = head
        while (fast.next !== null && fast.next.next !== null) {
            fast = fast.next.next
            slow = slow.next
        }
        return slow
    }
    // 找到前半部分链表的尾节点并反转后半部分链表
    const firstHalfEnd = endOfFirstHalf(head)
    const secondHalfStart = reverseList(firstHalfEnd.next)
    let p1 = head
    let p2 = secondHalfStart
    while (p2 != null) {
        if (p1.val != p2.val) return false
        p1 = p1.next
        p2 = p2.next
    }
    firstHalfEnd.next = reverseList(secondHalfStart)
    return true
}
```

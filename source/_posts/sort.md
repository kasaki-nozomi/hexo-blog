---
title: 排序相关算法 JS 实现
date: 2022-09-24 16:40:22
description: 用 JS 实现各种排序算法
categories:
-  JavaScript
tags:
-  JavaScript
-  数据结构
---

## BEFORE

排序算法分为两大类：

- **比较类排序**：通过比较来决定元素间的相对次序，其时间复杂度不能突破 <b><code>O(nlogn)</code></b>，因此也称为非线性时间比较类排序
	- 交换排序：冒泡排序、快速排序
	- 插入排序：简单插入排序、希尔排序
	- 选择排序：简单选择排序、堆排序
	- 归并排序：二路 / 多路归并排序

- **非比较类排序**：不通过比较来决定元素间的相对次序，可以突破基于比较排序的时间下界，以线性时间运行，因此也称为线性时间非比较类排序 
	- 基数排序
	- 计数排序
	- 桶排序


| 排序方法 | 平均时间复杂度 | 最坏时间复杂度 | 空间复杂度 | 稳定性 |
| :------: | :------------: | :------------: | :--------: | :----: |
| 冒泡排序 |     O(n^2)     |     O(n^2)     |    O(1)    |  稳定  |
| 选择排序 |     O(n^2)     |     O(n^2)     |    O(1)    | 不稳定 |
| 插入排序 |     O(n^2)     |     O(n^2)     |    O(1)    |  稳定  |
| 快速排序 |    O(nlogn)    |     O(n^2)     |  O(logn)   | 不稳定 |
|  堆排序  |    O(nlogn)    |    O(nlogn)    |    O(1)    | 不稳定 |
| 归并排序 |    O(nlogn)    |    O(nlogn)    |    O(n)    |  稳定  |
| 希尔排序 |  O(n^(1.3-2))  |     O(n2)      |    O(1)    | 不稳定 |
| 计数排序 |    O(n + k)    |    O(n + k)    |    O(k)    |  稳定  |
|  桶排序  |    O(n + k)    |     O(n^2)     |  O(n + k)  |  稳定  |
| 基数排序 |    O(n x k)    |    O(n x k)    |  O(n + k)  |  稳定  |

- **稳定**：`a` 原本在 `b` 前面，而 `a = b`，排序之后 `a` 仍在 `b` 的前面
- **不稳定**：排序之后 `a` 可能会出现在 `b` 的后面
- **时间复杂度**：对排序数据的总的操作次数，反映当 `n` 变化时，操作次数呈现什么规律
- **空间复杂度**：是指算法在计算机内执行时所需存储空间的度量，它也是数据规模 `n` 的函数

## 冒泡排序（Bubble Sort)

重复地走访要排序的数列，一次比较两个元素，若顺序错误则交换，重复地进行直到不再需要交换，则数列已排序完成

- 比较相邻的元素，若第一个比第二个大，交换
- 对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，在最后的元素应该会是最大的数
- 重复以上的步骤

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-19.gif" />

```javascript
const bubbleSort = function (array) {
    const len = array.length
    for (let i = 0; i < len; i++) {
        for (let j = 0; j < len - i - 1; j++) {
            if (array[j] > array[j + 1]) {
                [array[j], array[j + 1]] = [array[j + 1], array[j]]
            }
        }
    }
}
```
数组本身有序仅 `n-1` 次比较即完成，数组倒序，则比较次数为 `n-1+n-2+...+1 = n(n-1)/2`，时间复杂度为 <b><code style="color:#dd00dd">O(n^2)</code></b>，综合来说性能稍差于选择排序

## 选择排序（Selection Sort）

在未排序序列中找到最小（大）元素，存放到起始位置，再从剩余未排序元素中寻找最小（大）元素，存放到已排序序列的末尾......直到所有元素排序完毕

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-20.gif" />

```javascript
const selectionSort = function (array) {
    const len = array.length
    let minPos = 0
    for (let i = 0; i < len - 1; i++) {
        minPos = i
        for (let j = i + 1; j < len; j++) {
            if (array[j] < array[minPos]) {
                minPos = j
            }
        }
        [array[i], array[minPos]] = [array[minPos], array[i]]
    }
}
```
稳定的排序算法之一，无论如何都是 `O(n2)` 时间复杂度，不占用额外的内存空间，最好情况下数组完全有序的时候，`0` 交换次数，最差情况下，数组倒序的时候，交换次数为 `n-1` 次
综合下来，时间复杂度为 <b><code style="color:#dd00dd">O(n^2)</code></b>，空间复杂度为 <b><code style="color:#dd00dd">O(1)</code></b>

## 插入排序（Insertion Sort）

构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置插入

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-21.gif" />

```javascript
const insertionSort = function (array) {
    const len = array.length
    let position = 0
    let value = null
    for (let i = 1; i < len; i++) {
        position = i - 1
        value = array[i]
        while (position >= 0 && array[position] > value) {
            array[position + 1] = array[position]
            position--
        }
        array[position + 1] = value
    }
}
```
最好情况下，比较 `n-1` 次，无需交换元素，时间复杂度为 `O(n)`，最坏情况下，时间复杂度 <b><code style="color:#dd00dd">O(n^2)</code></b>，空间复杂度 <b><code style="color:#dd00dd">O(1)</code></b>，数组元素随机排列情况下，插入排序优于冒泡与选择

## 快速排序（Quick Sort）

一趟排序将数组分隔为独立的两部分，一部分小值一部分大值，可分别对这两部分记录继续进行排序，以达到整个序列有序

- 挑出一个元素，称为：**基准（pivot）**
- 排序数列，比基准值小的放在基准前面，比基准值大的放在基准后面（相同可任一边）：**分区（partition）**操作
- 递归地把小于基准值元素的子数列和大于基准值元素的子数列快速排序

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-23.gif" />

```javascript
const quickSort = function (array) {
    if (array.length <= 1) {
        return array
    }
    const len = array.length
    const pivot = array[0]
    const left = []
    const right = []
    for (let i = 1; i < len; i++) {
        array[i] < pivot ? left.push(array[i]) : right.push(array[i])
    }
    return quickSort(left).concat(pivot, quickSort(right))
}
```

```javascript
const partition = function (array, low, high) {
    const pivot = array[low]
    while (low < high) {
        while (array[high] >= pivot && low < high) high--
        array[low] = array[high]
        while (array[low] <= pivot && low < high) low++
        array[high] = array[low]
    }
    array[low] = pivot
    return low
}
const quickSort = function (array, low, high) {
    if (low < high) {
        const pos = partition(array, low, high)
        quickSort(array, low, pos - 1)
        quickSort(array, pos + 1, high)
    }
}
quickSort(array, 0, array.length-1)
```

快速排序的最差时间复杂度和冒泡排序是一样的都是 <b><code style="color:#dd00dd">O(n^2)</code></b>，它的平均时间复杂度为 <b><code style="color:#dd00dd">O(nlogn)</code></b>

## 希尔排序（Shell Sort）

将记录按下标一定增量分组，对每组使用**插入**排序，增量逐渐减少，增量减至 `1` 时，所有元素被分为一组，算法终止

在 `1959` 年 `Shell` 发明，第一个突破 `O(n2)` 的排序算法，简单插入排序改进版，与插入排序不同，优先比较距离较远的元素，希尔排序又叫**缩小增量排序**

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-22.gif" />

```javascript
const shellSort = function (array) {
    const len = array.length
    let gap = Math.floor(len / 2)
    while (gap >= 1) {
        for (let i = 0; i < len; i++) {
            for (let j = i; j >= gap; j = j - gap) {
                if (array[j] < array[j - gap]) {
                    [array[j], array[j - gap]] = [array[j - gap], array[j]]
                }
            }
        }
        gap = Math.floor(gap / 2)
    }
}
```
希尔排序中对于增量序列的选择十分重要，直接影响到希尔排序的性能，最坏时间复杂度为 <b><code style="color:#dd00dd">O(n^2)</code></b>，经过优化的增量序列可使得最坏时间复杂度为 <b><code style="color:#dd00dd">O(n^3/2)</code></b>，空间复杂度 <b><code style="color:#dd00dd">O(1)</code></b>

## 归并排序（Merge Sort）

建立在归并操作上的排序算法，采用分治法，将有序的子序列合并，得到有序的序列：先使每个子序列有序，再使子序列段间有序
若将两个有序序列合并成一个，称为 **2-路归并**

- 把长度为 `n` 的输入序列分成两个长度为 `n/2` 的子序列
- 对两个子序列采用归并排序
- 将两个排序好的子序列合并成一个最终的排序序列

多路归并同理

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-24.gif" />

```javascript
const merge = function (left, right) {
    const merged = []
    while (left.length && right.length) {
        merged.push(left[0] <= right[0] ? left.shift() : right.shift())
    }
    return merged.concat(left, right)
}
const mergeSort = function (array) {
    if (array.length <= 1) return array
    const len = array.length
    const mid = Math.floor(len / 2)
    return merge(mergeSort(array.slice(0, mid)), mergeSort(array.slice(mid)))
}
```
稳定的排序方法，性能不受输入数据的影响，但表现比选择排序好，时间复杂度始终是 <b><code style="color:#dd00dd">O(nlogn)</code></b>，但需要额外的内存空间，空间复杂度为 <b><code style="color:#dd00dd">O(n)</code></b>

## 堆排序（Heap Sort）

堆排序 **<code>HeapSort</code>** 是指利用堆这种数据结构所设计的一种排序算法，堆是一个近似完全二叉树的结构，并同时满足堆的性质：即子结点的键值或索引总是小于（或者大于）它的父节点

- 将初始待排序关键字序列 `R1,R2…,Rn` 构建成大顶堆，此堆为初始的无序区
- 将堆顶元素 `R[1]` 与最后元素 `R[n]` 交换，得到新无序区 `R1,R2,……Rn-1` 和新有序区 `Rn`，且满足 `R[1,2…n-1] <= R[n]`
- 交换后新的堆顶 `R[1]` 可能违反堆性质，需对当前无序区 `R1,R2,……Rn-1` 调整为新堆，将 `R[1]` 与无序区最后一个元素交换，得到新无序区 `R1,R2….Rn-2` 和新的有序区 `Rn-1,Rn`，不断重复此过程直到有序区的元素个数为 `n-1`

<img width="600" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-25.gif" />

```javascript
const heapSort = function (array) {
    let len = array.length
    for (let i = Math.floor(len / 2); i >= 0; i--) {
        heapify(array, i)
    }
    for (let i = len - 1; i > 0; i--) {
        [array[0], array[i]] = [array[i], array[0]]
        len--
        heapify(array, 0)
    }
    function heapify(array, index) {
        const left = 2 * index + 1
        const right = 2 * index + 2
        let large = index
        if (left < len && array[left] > array[large]) large = left
        if (right < len && array[right] > array[large]) large = right
        if (large !== index) {
            [array[index], array[large]] = [array[large], array[index]]
            heapify(array, large)
        }
    }
    return array
}
```

## 计数排序（Counting Sort）

计数排序不是基于比较的排序算法，核心在于将输入的数据值转化为键存储在额外开辟的数组空间中

作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数

- 找出待排序的数组中最大和最小的元素
- 统计数组中值为 `i` 的元素出现的次数，存入数组 `C` 的第 `i` 项
- 对所有的计数累加（从 `C` 中的第一个元素开始，每一项和前一项相加）
- 反向填充目标数组：将每个元素 `i` 放在新数组的第 `C(i)` 项，每放一个元素就将 `C(i)` 减去 `1`

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-26.gif" />

```javascript
const countingSort = function (array, maxValue) {
    const len = array.length
    const countArray = new Array(maxValue + 1).fill(0)
    let index = 0
    for (let i = 0; i < len; i++) {
        countArray[array[i]]++
    }
    for (let j = 0; j < maxValue + 1; j++) {
        while (countArray[j]) {
            array[index++] = j
            countArray[j]--
        }
    }
}
```

计数排序是**稳定**的排序算法，输入的元素是 `n` 个 **<code>0 - k</code>** 之间的整数时，时间复杂度是 <b><code style="color:#dd00dd">O(n+k)</code></b>，空间复杂度也是 <b><code style="color:#dd00dd">O(n+k)</code></b>，排序速度快于任何比较排序算法，当 `k` 不是很大并且序列比较集中时，计数排序是很有效的排序算法

## 桶排序（Bucket Sort）

桶排序是计数排序的升级版，利用了函数的映射关系，高效与否关键就k在于映射函数的确定

桶排序 **<code>Bucket sort</code>** 的工作的原理：假设输入数据服从均匀分布，将数据分到有限数量的桶里，每个桶再分别排序（有可能再使用别的排序算法或是以递归方式继续使用桶排序进行）

- 设置一个定量的数组当作空桶
- 遍历输入数据，并且把数据一个一个放到对应的桶里去
- 对每个不是空的桶进行排序
- 从不是空的桶里把排好序的数据拼接起来

```javascript
const bucketSort = function (array, bucketSize) {
    const DEFAULT_BUCKET_SIZE = 5
    bucketSize = bucketSize || DEFAULT_BUCKET_SIZE
    const len = array.length
    const minValue = Math.min(...array)
    const maxValue = Math.max(...array)
    const bucketCount = Math.floor((maxValue - minValue) / bucketSize) + 1
    const buckets = new Array(bucketCount).fill(0).map(() => new Array())
    for (let i = 0; i < len; i++) {
        buckets[Math.floor((array[i] - minValue) / bucketSize)].push(array[i])
    }
    array.length = 0
    for (let j = 0, bucketLength = buckets.length; j < bucketLength; j++) {
        insertSort(buckets[j])
        array.push(...buckets[j])
    }
    return array
}
function insertSort(array) {
    const len = array.length
    let position = 0
    let value = 0
    for (let i = 1; i < len; i++) {
        position = i - 1
        value = array[i]
        while (position >= 0 && array[position] > value) {
            array[position + 1] = array[position]
            position--
        }
        array[position + 1] = value
    }
}
```

桶排序**最好情况**下使用线性时间 <b><code style="color:#dd00dd">O(n)</code></b>，桶排序的时间复杂度，取决与对各个桶之间数据进行排序的时间复杂度，因为其它部分的时间复杂度都为 <b><code>O(n)</code></b>，桶划分的越小，各个桶之间的数据越少，排序所用的时间也会越少，但相应的空间消耗就会增大

## 基数排序（Radix Sort）

基数排序是按照低位先排序，然后收集，再按照高位排序，然后再收集，依次类推，直到最高位
有些属性是有优先级顺序的，先按低优先级排序，再按高优先级排序，最后的次序就是高优先级高的在前，高优先级相同的低优先级高的在前

- 取得数组中的最大数，并取得位数
- 从最低位开始取每个位组成 `radix` 数组
- 对 `radix` 进行计数排序（利用计数排序适用于小范围数的特点）

<img width="800" src="https://raw.githubusercontent.com/kasaki-nozomi/Sources/main/Images/image-27.gif" />

```javascript
function radixSort(array, maxDigit) {		// maxDigit 为最大位数
    const len = array.length7
    const counter = new Array(10).fill(0).map(() => new Array())
    for (let i = 0, dev = 1, mod = 10; i < maxDigit; i++, dev *= 10, mod *= 10) {
        for (let j = 0; j < len; j++) {
            const bucket = Math.floor((array[j] % mod) / dev)
            counter[bucket].push(array[j])
        }
        array.length = 0
        for (let k = 0; k < 10; k++) {
            if (counter[k].length) {
                array.push(...counter[k])
                counter[k].length = 0
            }
        }
    }
    return array
}
```

基数排序基于分别排序，分别收集，是**稳定**的，基数排序**性能比桶排序要略差**，每一次关键字的分配都需要 **<code>O(n)</code>** 的时间复杂度，分配之后得到新的关键字序列也需要 **<code>O(n)</code>** 的时间复杂度。假如待排数据可以分为 **<code>d</code>** 个关键字，则基数排序的**时间复杂度**将是 <b><code style="color:#dd00dd">O(2n*d)</code></b> ，当然 `d` 要远远小于 `n`，因此基本上还是线性级别的

基数排序**空间复杂度**为 <b><code style="color:#dd00dd">O(n+k)</code></b>，`k` 为桶的数量。一般来说 `n >> k`，因此额外空间需要大概 `n` 个左右k

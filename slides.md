---
theme: penguin
layout: intro
---

# 聊一聊递归

---

## 先来讲一个故事

<v-clicks>

我给你讲个故事哈，从前有座山，山上有座庙，庙里有个老和尚和一个小和尚，有一天，老和尚对小和尚说：“从前有座山，山上有座庙，庙里有个老和尚和一个小和尚，有一天，老和尚对小和尚说……”

<div class="mt-[120px] mb-[20px]">
这个故事变成代码会是什么样子呢？
</div>

```ts
function tellStory() {
  console.log("从前有座山，山上有座庙，庙里有个老和尚和一个小和尚，有一天，老和尚对小和尚说:" + tellStory())
}
 
console.log("我给你讲个故事哈" + tellStory())
```

</v-clicks>

---

## 执行这个代码会怎么样呢?

<v-clicks>

<div class="mt-[120px] mb-[20px]">

![Local Image](/stack-overflow.png)
<br>
这个错误也叫 Stack Overflow。哎，有个网址好像叫 https://stackoverflow.com/ 

那么问题来了，为啥出现栈溢出。在解释这个问题之前我们先来了解下 “stack” 栈、"call stack" 调用栈
</div>

</v-clicks>

---

## 什么是栈，什么是调用栈

<v-clicks>

栈是一种抽象数据类型，充当元素的集合，具有两个主要操作:push和pop
<img src="/stack.png" class="mt-10 h-80 rounded shadow" />

<div class="text-red-500">大家有没有发现一个特性，先进去的元素后出来，后进去的元素先出来（LIFO，last in first out）</div>
</v-clicks>

---

## 调用栈

<v-clicks>
<div class="mt-[20px]">
<div v-if="$slidev.nav.clicks < 2">
  调用栈也是栈，只不过栈中的元素稍微复杂点（大概包括了全局上下文 global，执行上下文 fec）

  下面举个例子来详细看下 call stack 的过程
</div>

<div>

```ts
function goodbye() {
  console.log('bye')
}
function sayHi() {
  console.log('hi')
  goodbye();
}
sayHi()
```
</div>
</div>

<div class="mt-[10px] w-160">
step1: 当浏览器加载完 js 后就会开始执行 js ，此时会创建一个匿名元素并放入 call stack 中
</div>
<div class="mt-[10px] w-160">
step2: 执行到 sayHi 时会创建一个叫 sayHi 的元素放入 call stack 中
</div>
<div class="mt-[10px] w-160">
step3: sayHi 函数中调用了 goodbye 函数，当执行到调用这行时会创建个叫 goodbye 的元素放入 call stack 中
</div>
<div class="mt-[10px] w-160">
step4: 进入 goodbye 函数，执行完 console.log 后，这个函数就没有需要执行的语句了。此时会从 call stack 把 goodbye 元素弹出，返回到 sayHi 中继续执行
</div>
<div class="mt-[10px] w-160">
step5: 继续执行 sayHi 当 sayHi 中没有语句需要执行时，会从 call stack 中把 sayHi 弹出，返回到匿名(global)中执行
</div>
<div class="mt-[10px] w-160">
step6: 当 global 中没有语句执行时, call stack 就会把匿名元素弹出
</div>
</v-clicks>

<stack
  v-if="$slidev.nav.clicks >= 2"
  :step="$slidev.nav.clicks - 1"
  :mapper="{1: ['gec'], 2:['gec', 'sayHi'], 3: ['gec', 'sayHi', 'goodbye'], 4:['gec', 'sayHi'], 5: ['gec'], 6: []}"
></stack>

---

## 问题

现在我们对 stack 和 call stack 有了比较清晰的理解，那来回答我们之前遇到的问题：为啥会出现栈溢出？

---

## 什么是递归

<v-clicks>
<div class="mt-[20px]">
盗梦空间大家应该都看过吧，dreams within dreams.

还有一个关于递归的冷笑话，To understand recursion, you must first understand recursion
</div>

#### 定义：
<div>

递归是当过程的一个步骤涉及调用过程本身时，经过递归的过程称为“递归”。

那我们如何写一个递归呢，避免出现 call stack 这样的错误。

一个健康的递归应该包括两部分：

- base case
- recursive case
</div>
</v-clicks>

---

## 斐波那契数

1，1，2，3，5，8，13，21，34 。。。

- F(0) = 1
- F(1) = 1
- F(n) = F(n-1) + F(n-2)

<v-clicks>
<div></div>
<div></div>
<div></div>
</v-clicks>

<div v-if="$slidev.nav.clicks === 1">

```ts
function f(n) {
  if (n === 0 || n === 1) {
    return 1
  }
  return f(n-1) + f(n-2);
}
f(4) // 5
```
</div>

<div v-if="$slidev.nav.clicks === 2">

```ts {2,3,4}
function f(n) {
  // base case
  if (n === 0 || n === 1) {
    return 1
  }
  return f(n-1) + f(n-2);
}
f(4) // 5
```
</div>

<div v-if="$slidev.nav.clicks === 3">

```ts {5,6}
function f(n) {
  if (n === 0 || n === 1) {
    return 1
  }
  // recursive case
  return f(n-1) + f(n-2);
}
f(4) // 5
```
</div>

---

## 再来看看执行 f(4) 时的执行流程和 call stack

<div class="mt-[40px] flex">
<img src="/f-flow.png" class="h-70 rounded shadow" />
<img src="/f-stack.png" class="ml-10 h-80 rounded shadow" />
</div>

<v-clicks>

<div class="mt-[10px]">
如何用递归解决特定的问题呢？
</div>
<div class="mt-[10px]">
递归解决问题的一个思路是：假如我要做到 n，我假设 n-1 已经做到了，那我想达到 n 我需要做什么。这里的做什么就是递归函数要做的事。
</div>
<div class="mt-[10px]">
例子
</div>
</v-clicks>

---

## 如何实现字符串的反转

<v-clicks >
<div class="mt-[20px]">
假设我要 n-1 前面的字符都是反转好的，现在我要反转第 n 个字母，我需要做什么
</div>

<div class="mt-[20px]">
是不是就把第 n 个字符和 n-1 的结果做个对调就好啦
</div>

<div class="mt-[20px]">

```ts
function reverse(str) {
  if (str.length === 1 || str.length === 0) {
    return str
  }
  return `${str[str.length -1]}${reverse(str.slice(0, str.length-1))}`
}
```
</div>
</v-clicks>

---

## 二叉树深度优先遍历

<v-clicks>
<div></div>
<div></div>
</v-clicks>

<div v-if="$slidev.nav.clicks === 1">
<img src="/tree.png" class="mt-10 h-70 rounded shadow" />
打印 DBEAFCG
</div>

<div v-if="$slidev.nav.clicks === 2">
打印 DBEAFCG（中序遍历首先遍历左子树，然后访问根结点，最后遍历右子树）

<p>我们假设有一条线画在根节点上[[D|B|E]|A|[F|C|G]], 对于每个根节点都是先打印了左子树，然后打印根节点，然后打印右子树</p>
那我们把 n-1 完成的事情看成是 n 层的节点的左右子节点已经完成了打印，现在我们只需要做什么，完成根节点的打印。
<div style="overflow: auto;height:350px">

```ts
const tree = {
  node: 'A',
  left: {
    node: 'B',
    left: {
      node: 'D'
    },
    right: {
      node: 'E'
    }
  },
  right: {
    node: 'C',
    left: {
      node: 'F'
    },
    right: {
      node: 'G'
    }
  }
}
 
function DFSTree(tree) {
  if (tree.left) {
    DFSTree(tree.left)
  }
  console.log(tree.node)
  if (tree.right) {
    DFSTree(tree.right)
  }
}
DFSTree(tree)
```
</div>
</div>

---

## 总结下

<v-clicks>
<div class="mt-[20px]">
我们知道了递归函数的执行过程，我们就能很好的看懂别人写的递归代码
</div>
<div class="mt-[20px]">
我们知道了如何用递归函数解决特定问题的思路（已知 n-1 解决 n）
</div>
</v-clicks>

---

## 递归和循环的对比
<div class="mt-10"></div>

#### 如何实现字符串的反转 reverse

<v-clicks>
<div class="mt-10">
非递归版

```ts
function reverse(str) {
  let newstr = '';
  for(let i = str.length - 1; i >= 0; i--) {
    newstr = `${newstr}${str[i]}`
  }
  return newstr;
}
```
</div>
<div class="mt-10">
递归版

```ts
function reverse(str) {
  if (str.length === 1 || str.length === 0) {
    return str
  }
  return `${str[str.length -1]}${reverse(str.slice(0, str.length-1))}`
}
```
</div>
</v-clicks>


---

## 递归和循环的对比
<div class="mt-10"></div>

#### 斐波那契数非递归版本

<v-clicks>

```ts
function f(n) {
  if (n == 0 || n == 1) return 1
  let f1 = f2 = 1;
  let f3
  for (let i=2 ; i<=n; i++) {
    f3 = f1 + f2;
    f1 = f2;
    f2 = f3;
  }
  return f3
}
```

<div class="mt-10">
线性代数

[f(n), f(n-1)] = [f(0), f(1)] * [[1,1],[1,0]]^n-2
</div>
</v-clicks>

---

## 递归和循环的对比 --- 结论
<div class="mt-10"></div>

|            | 递归        | 循环 |
|  ----      | ----       | ----  |
| 代码量      | 一般比较小	 | 一般比较大 |
| 时间复杂度	 | 比较大      | 相对较低 |
| 空间复杂度	 | 额外栈开销   | 不需要额外栈 |

<div class="mt-10"></div>

一般情况推荐使用循环，不使用递归。解决特定问题时推荐使用递归，比如汉诺塔问题，DFS，类树结构问题。还有一些使用递归容易相对解决办法，可以先使用递归实现再转成非递归（很多动态规划问题就可以先实现递归方法再转成动态规划）

---

## 最后 -- 更全些的文档
<div class="mt-10"></div>

https://confluence.alauda.cn/pages/viewpage.action?pageId=124695318
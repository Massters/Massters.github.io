---
layout: blog
istop: false
basics: true
title: "数据结构重点"
date:  2022-08-17
category: DS
tags:
- DS
- 算法
---

# 小总结

## 算法

<style>table{text-align:center} </style>
<table>
   <tr>
      <td colspan="2">排序方法</td>
      <td>最好</td>
      <td>最坏</td>
      <td>平均</td>
      <td>空间复杂度</td>
      <td>稳定性</td>
      <td>适用</td>
      <td>备注</td>
   </tr>
   <tr>
      <td rowspan="3">插入</td>
      <td>直接插入</td>
      <td>O(n)</td>
      <td colspan="2">O(n<sup>2</sup>)</td>
      <td rowspan="4">O(1)</td>
      <td>√</td>
      <td align="left">顺序，链表</td>
      <td></td>
   </tr>
   <tr>
      <td>折半插入</td>
      <td>O(nlog<sub>2</sub>n)</td>
      <td colspan="2">O(n<sup>2</sup>)</td>
      <td>√</td>
      <td align="left">顺序</td>
      <td>先折半找插入位置再移动</td>
   </tr>
   <tr>
      <td>希尔</td>
      <td>-</td>
      <td>-</td>
      <td>-</td>
      <td>×</td>
      <td align="left">顺序</td>
      <td></td>
   </tr>
   <tr>
      <td rowspan="2">交换</td>
      <td>冒泡</td>
      <td>O(n)</td>
      <td colspan="2">O(n<sup>2</sup>)</td>
      <td>√</td>
      <td align="left">顺序，链表</td>
      <td></td>
   </tr>
   <tr>
      <td>快速</td>
      <td>O(nlog<sub>2</sub>n)</td>
      <td>O(n<sup>2</sup>)</td>
      <td>O(nlog<sub>2</sub>n)</td>
      <td>O(log<sub>2</sub>n)~O(n)</td>
      <td>×</td>
      <td align="left">顺序</td>
      <td></td>
   </tr>
   <tr>
      <td rowspan="2">选择</td>
      <td>简单选择</td>
      <td colspan="3">O(n<sup>2</sup>)</td>
      <td rowspan="2">O(1)</td>
      <td>×</td>
      <td align="left">顺序，链表</td>
      <td></td>
   </tr>
   <tr>
      <td>堆</td>
      <td colspan="3">O(nlog<sub>2</sub>n)</td>
      <td>×</td>
      <td align="left">顺序</td>
      <td>向下调整O(log<sub>2</sub>n),建堆O(n)</td>
   </tr>
   <tr>
      <td>归并</td>
      <td>二路归并</td>
      <td colspan="3">O(nlog<sub>2</sub>n)</td>
      <td>O(n)</td>
      <td>√</td>
      <td align="left">顺序</td>
      <td></td>
   </tr>
   <tr>
      <td>基数</td>
      <td>基数</td>
      <td colspan="3">O(d(r+n))</td>
      <td>O(r)</td>
      <td>√</td>
      <td></td>
      <td>与序列初始状态无关</td>
   </tr>
   <tr>
      <td colspan="2"></td>
      <td colspan="3">d趟，一趟收集O(r),一趟分配O(n)</td>
      <td>r为基数</td>
      <td></td>
      <td></td>
      <td></td>
   </tr>
</table>



注：

- 每趟确定一个元素的最终位置：

  - 冒泡，简单选择，堆$\implies$全局有序子序列
  - 快速

- 局部有序子序列（或叫基本有序）：直接插入

- 所有简单排序（直接插入，冒泡，简单选择）的时间复杂度：$O(n^2)$

- $O(n\log_2n)$：

  - 快速排序，堆$\implies$不稳定
  - 归并$\implies$稳定

  其中：
  - 快速排序可能出现最坏情况，时间复杂度$O(n^2)$空间复杂度$O(n)$
  - 堆：最好最坏平均都是$O(n\log_2n)$

## 二叉树

- 高h二叉树至多$2^h-1$个结点（$h \geqslant 1$）
- 非空二叉树k层至多$2^{k-1}$个结点($k\geqslant$1)
- 非空二叉树叶子结点数$n_0=n_2+1$
- n个结点完全二叉树高$\lceil\log_2(n+1)\rceil$或$\lfloor\log_2n\rfloor+1$

## 其他重要算法

- BST查找效率最好$O(\log_2N)$最坏$O(N)$
- BFS空间复杂度$O(\|V\|)$
  - 邻接矩阵：时间复杂度$O(\|V\|^2)$
  - 邻接表：时间复杂度$O(\|V\|+\|E\|)$
- DFS空间复杂度$O(\|V\|)$
  - 邻接矩阵：时间复杂度$O(\|V\|^2)$
  - 邻接表：时间复杂度$O(\|V\|+\|E\|)$
- Prim（选点，最小生成树） 时间复杂度：$O(\|V\|^2)$
- Kruskal(选边，最小生成树) 时间复杂度：$O(\|E\|\log_2\|E\|)$
- Dijkstra(最短路径) 时间复杂度$O(\|V\|^2)$
- Floyd 时间复杂度$O(\|V\|^3)$  允许有负权值边，但不能有负值边成回路
- 拓扑排序 时间复杂度 $O(\|V\| + \|E\|)$
- 顺序查找 时间复杂度 $O(n)$
- 折半查找 时间复杂度 $O(\log_2n)$
- 简单匹配 时间复杂度 $O(mn)$
- KMP 时间复杂度$O(m+n)$完成串的模式匹配


> 公式中的绝对值符号在Typora中显示为`||`,网页端显示正常~

<p align="right">2022-08-17</p>
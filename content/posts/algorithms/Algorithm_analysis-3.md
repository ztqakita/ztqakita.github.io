---
title: "Greedy & Back-track & Branch and Bound"
date: 2020-11-16T06:00:20+06:00
hero: /images/posts/DL/139-deep-learning.svg
menu:
  sidebar:
    name: Greedy & Back-track & Branch and Bound
    identifier: a3
    parent: Algorithms
    weight: 10
math: true
---

# 贪心算法
算法+**证明**
## 一、最优装载问题
1. 算法：
```C++
void Loading(int x[], int w[], int c, int n)
{
    int *t = new int [n+1];
    Sort(w, t, n);
    for(int i = 0; i < n; i++)
        x[i] = 0;
    for(int i = 0; i < n && w[t[i]] < c; i++)
    {
        x[t[i]] = 1;
        c -= w[t[i]];
    }
}
```

2. 证明：
   - 最优子结构性质：


## 二、哈夫曼编码
```C++

```


## 三、最小生成树

# 回溯法
子集树：0-1背包问题，从包含n个全集当中去选择一个子集，所有可能解是$O(2^n)$
排序树：TSP问题，解是一个排列，所有可能解的规模是$O(n!)$
- 子集树：当所给的问题是从n个元素的集合S中找出满足某种性质的子集时，相应的解空间称为子集树。
- 遍历子集树
时间复杂度：$O(2^n)$
```C++
void backtrack (int t)
{
    if (t>n) 
        output(x);
    else
    {
      for (int i=0;i<=1;i++) 
      {
        x[t]=i;
        if (legal(t)) 
            backtrack(t+1);
      }
    }
}
```
- 排列树：当所给的问题是确定n个元素满足某种性质的排列时，相应的解空间树成为排列树。
- 遍历排序树
时间复杂度：$O(n!)$
```C++
void backtrack (int t)
{
    if (t>n) output(x);
    else {
      for (int i=t;i<=n;i++) {
        swap(x[t], x[i]);
        if (legal(t)) backtrack(t+1);
        swap(x[t], x[i]);
      }
    }
} 
```

剪枝设计：
1. 问题的约束条件不满足时
2. 当大于当前的最优解时
3. 在构造排序树时，为了达到很好的剪枝效果，深度遍历的第一个叶子节点是由贪心方法得出的。
贪心方法+分支限界：效果很好

## 1. 装载问题

## 2. 批处理作业调度问题

回溯法考试题目步骤：
设计出解向量
画出解空间树
给出剪枝的方案
写出伪代码
画出剪枝

## 3. n皇后问题
此时的解空间树不再是传统的子集树、排序树，而是自己独有的树。

## 4. 图的着色问题
1. 解空间：
n维向量，每一维代表一个顶点，值代表其的颜色值。
2. 解空间树
根节点从顶点1开始，

3. 迭代方式：
```C++
GraphColor(int n,int m,int color[],bool c[][5])
{
     int i,k;  
    for (i=0; i<n; i++ )           //将解向量color[n]初始化为0
        color[i]=0;
    k=0;
    while (k>=0)
    {   
        color[k]=color[k]+1;       //使当前颜色数加1
        while ((color[k]<=m) && (!ok(color,k,c,n))) //当前颜色是否有效
        color[k]=color[k]+1;   //无效，搜索下一个颜色
        if (color[k]<=m)       //求解完毕，输出解
        {
            if (k==n-1)
              break;  //是最后的顶点，完成搜索
            else 
              k=k+1;        //否，处理下一个顶点
        }
        else                   //搜索失败，回溯到前一个顶点
        {
    color[k]=0;
	    k=k-1;
          }
     }
}
```

4. 递归方式：
```C++
void GraphColor(int k)
{
    if(k > n)
    {
      sum++;
      for(int i = 1; i <= n; i++)
        cout << x[i] << " ";
      cout << endl;
    }
    else
    {
      for(int i = 1; i <= m; i++)
      {
        x[k] = i;
        if(ok(k))
          GraphColor(k+1);
      }
    }
}

void ok(int k)
{
  for(int i = 1; i <= n; i++)
  {
    if((c[i][k]==1) && (x[i]==x[k]))
      return false;
    return true;
  }
}
```


# 分支限界法
![](/images/posts/algo/17.JPG)
常用的分支限界法
1. 队列式(FIFO)分支限界法
2. 优先队列式分支限界法
   
## 1. 0/1背包问题

上界的作用：叶子结点得到的当前最优值可以剪去很多在优先队列中尚未拓展的结点，与这些结点的理想值ub进行比较再删减。

- 先进先出队列：每次队列中总是取出头元素，不考虑元素间的大小关系；
- 优先队列：每次取出的头元素均是经过比较后得到的队列中值最优的元素。更为常用

## 2. TSP问题
1. 用贪心法得到的近似解作为问题的**上限**，从而进行剪枝；
2. 考虑TSP一个完整的解，每一个城市都会进去一次，出来一次，所以从邻接矩阵中每一行取出两个最小的元素(代表从这个城市进出的最小代价)，此处牢记若已经有确定的边加入，就要加上该边和其他权值最小的边。这样**加起来以后除以2**，就得到下界。


以大题为主，分支限界法不考，

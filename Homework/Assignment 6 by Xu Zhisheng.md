# Assignment #6: Recursion and DP

Updated Nov 1, 2024

2024 fall, Complied by 徐至晟，光华



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### sy119: 汉诺塔

recursion, https://sunnywhy.com/sfbj/4/3/119  

思路：

首先输出步数：$2^n-1$

$n个圆盘从A移到C \Leftrightarrow 上面n-1个圆盘从A移到B\rightarrow大圆盘从A移到C\rightarrow上面n-1个圆盘从B移到C$

递归时区分起始盘、终止盘、中转盘

Python中要调用的函数定义在调用行前面

代码：

```python
def hanoi(k,st,ed,mi):
    if k==1:
        print(st,'->',ed,sep='')
        return
    hanoi(k-1,st,mi,ed)
    hanoi(1,st,ed,mi)
    hanoi(k-1,mi,ed,st)


n=int(input())
print((1<<n)-1)
hanoi(n,'A','C','B')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/bvhoqg3t.png)



### sy132: 全排列I

recursion, https://sunnywhy.com/sfbj/4/3/132

思路：

DFS，标记当前选过的数字。递归前后分别需要添加、去除标记，修改与还原当前答案。

代码：

```python
n=int(input())
tot=0
v=[False]*(n+3)
u=[]
m=1
for i in range(2,n+1):
    m*=i

def escribir():
    global u,n
    print(u[0],end='')
    for j in range(1,n):
        print('',u[j],end='')
    print('')

def hacer(k):
    global u,v,tot,n,m
    if tot>m:
        return
    if k==0:
        tot+=1
        escribir()
        return
    for i in range(1,n+1):
        if not v[i]:
            v[i]=True
            u.append(i)
            hacer(k-1)
            u.pop()
            v[i]=False

hacer(n)
```



代码运行截图 ==（至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/1t4aui4h.png)



### 02945: 拦截导弹 

dp, http://cs101.openjudge.cn/2024fallroutine/02945

思路：

求最长不下降子序列。

设$f_i$表示选第$i$枚导弹，前$i$枚导弹中拦截的最大数量。

注意初始设定$f_i=1(i\in[1,n])$

若存在合法的$j$则更新：$f_i=\max\limits_{0<j<i且h_j\geq h_i}{\left\{ f_j+1 \right\}}$

时间复杂度$O(n^2)$

代码：

```python
n=int(input())
h=list(map(int,input().split()))
f=[1]
if n==0:
    print(0)
else:
    ans=1
    for i in range(1,n):
        fi=1
        for j in range(0,i):
            if h[j]>=h[i]:
                fi=max(fi,f[j]+1)
        ans=max(fi,ans)
        f.append(fi)
    print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/zvs03qe2.png)



### 23421: 小偷背包 

dp, http://cs101.openjudge.cn/practice/23421

思路：

动态规划经典模型01背包。一维数组优化空间，背包容量$j$倒序循环，表示每个物品只取一次。

代码：

```python
n,b=map(int,input().split())
v=list(map(int,input().split()))
w=list(map(int,input().split()))
f=[int(-1e9)]*(b+5)
f[0]=0
for i in range(1,n+1):
    for j in range(b,w[i-1]-1,-1):
        f[j]=max(f[j],f[j-w[i-1]]+v[i-1])
ans=int(-1e9)
for j in range(0,b+1):
    if ans<f[j]:
        ans=f[j]
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/iq6zt87c.png)



### 02754: 八皇后

dfs and similar, http://cs101.openjudge.cn/practice/02754

思路：

如何记录同一行（不用记录）、列、对角线有没有棋子？

对角线看点的横纵坐标相加减

神奇的现象：如果以下代码3-5行写成

```python
v1=v2=v3=[False]*21
```

编译器认为`v1,v2,v3`代表同一个变量，而不是三个变量😂



代码：

```python
res=[]
ahora=[]
v1=[False]*21
v2=[False]*21
v3=[False]*21
def numero(palabra):
    num=0
    for j in palabra:
        num=num*10+j
    return num

def buscar(t):
    global res,ahora,v1,v2,v3
    if t>8:
        res.append(numero(ahora))
        return
    for i in range(1,9):
        if not (v1[i] or v2[i+t] or v3[i-t+9]):
            v1[i]=v2[i+t]=v3[i-t+9]=True
            ahora.append(i)
            buscar(t+1)
            ahora.pop()
            v1[i]=v2[i+t]=v3[i-t+9]=False
    
n=int(input())
buscar(1)
while n:
    k=int(input())
    print(res[k-1])
    n-=1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/ygpifo5a.png)



### 189A. Cut Ribbon 

brute force, dp 1300 https://codeforces.com/problemset/problem/189/A

思路：

硬币组成面值问题，不过本题只有三个物品。

注意赋初值$f[0]=0,f[x]=+\infty(x\in N^*)$，因为要排除不能凑出相应面值的情况，不能全部赋值为0

代码：

```python
n,a,b,c=map(int,input().split())
f=[-(1<<32)]*(n+3)
f[0]=0
for i in range(min(a,b,c),n+1):
    if i>=a:
        f[i]=max(f[i],f[i-a]+1)
    if i>=b:
        f[i]=max(f[i],f[i-b]+1)
    if i>=c:
        f[i]=max(f[i],f[i-c]+1)
print(f[n])
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/s5sky3r9.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

复习回忆DFS和DP的基本模型和经典题目。但DP的初始值、边界条件需要认真考虑。




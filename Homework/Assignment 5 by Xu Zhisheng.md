# Assignment #5: Greedy穷举Implementation



2024 fall, Complied by 徐至晟，光华



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 04148: 生理周期

brute force, http://cs101.openjudge.cn/practice/04148

思路：

没有想到好的数学方法。枚举当前日期开始的1至21252天，判断与三个高峰的天数差是否分别为23, 28, 33的倍数。

代码：

```python
n=1
while True:
    p,e,i,d=map(int,input().split())
    if p<0:
        break
    for t in range(1,21253):
        d+=1
        if (d-p)%23==0 and (d-e)%28==0 and (d-i)%33==0:
            print('Case %d: the next triple peak occurs in %d days.'%(n,t))
            break
    n+=1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/pw4pn4gr.png)



### 18211: 军备竞赛

greedy, two pointers, http://cs101.openjudge.cn/practice/18211

思路：



代码：

```python
p=int(input())
a=list(map(int,input().split()))
a.sort()
i=0
j=len(a)-1
c1=c2=delta=0
ans=0
while i<=j:
    if p<a[i]:
        break
    while i<=j and p>=a[i]:
        p-=a[i]
        delta+=1
        i+=1
    ans=max(ans,delta)
    if i<j:
        if delta>0:
            p+=a[j]
            delta-=1
            j-=1
    else:
        break
print(ans)
```



代码运行截图 ==（至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/7q6q9jyf.png)



### 21554: 排队做实验

greedy, http://cs101.openjudge.cn/practice/21554

思路：

小学奥数题，把实验时间少的人放前面。对于排序后的第$i$个人，贡献的总等待时间为$t_i\times (n-i)$。

**题目中人的下标是从1开始，而不是从0开始！**

代码：

```python
n=int(input())
a=list(map(int,input().split()))
b=[]
for i in range(0,n):
    b.append((a[i],i))
b.sort()
tot=0.0
for i in range(0,n):
    tot+=b[i][0]*(n-i-1)
    print(b[i][1]+1,'',end='')
print('')
print('%.2f'% (tot/n))
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/ppzh8zx7.png)



### 01008: Maya Calendar

implementation, http://cs101.openjudge.cn/practice/01008/

思路：

只有单方向，不要求互相转换，难度下降。如果反向转换难度会大一些。

以创世纪为第0天，根据 Haab 历法算出当前日期是第$D$天（题中每月几号是从0开始），然后换算成 Tzolkin 历法。年份用$D$整除260，“干支”分别对$(D \mod 260)$取模13和20得到。

代码：

```python
haab={'pop':0,'no':1,'zip':2,'zotz':3,
      'tzec':4,'xul':5,'yoxkin':6,'mol':7,
      'chen':8,'yax':9,'zac':10,'ceh':11,
      'mac':12,'kankin':13,'muan':14,'pax':15,
      'koyab':16,'cumhu':17,'uayet':18}
tz=['imix','ik','akbal','kan',
    'chicchan','cimi','manik','lamat',
    'muluk','ok','chuen','eb',
    'ben','ix','mem','cib',
    'caban','eznab','canac','ahau']
n=int(input())
print(n)
while n:
    day, mon, yr=map(str,input().split())
    day=int(day[0:-1])
    yr=int(yr)
    D=yr*365+haab[mon]*20+day
    year=D//260
    date=D-year*260
    print(date%13+1,tz[date%20],year)
    n-=1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/a7bdmh8m.png)



### 545C. Woodcutters

dp, greedy, 1500, https://codeforces.com/problemset/problem/545/C

思路：

动态规划。原题输入已经按树的坐标从小到大排序。设$F_{i,j}$表示处理完前$i$棵树，第$i$棵树采取第$j$种方法（0：不砍，1：砍树向左倒下，2：砍树向右倒下），砍树的最大数目。

第$i-1$棵树一定是目前最右边的一棵树，因为砍了前面的树不能占用它的空间。

因为$F_i$的状态只会用到$F_{i-1}$更新，可以用两个大小为$3$的数组代替$n\times 3$数组，$g$表示$F_{i-1}$，$f$表示$F_i$。

逐个判断当前砍树是否合法，具体实现见代码。

调试时一定检查$n=1$时候的情况！

代码：

```python
n=int(input())
x=[]
h=[]
f=[0,0,0]
g=[0,0,0]
for i in range(0,n):
    z, y=map(int,input().split())
    x.append(z)
    h.append(y)
x.append(2000000020)
h.append(0)
#方便判断最后一棵树往右砍，防止越界
g[0]=0
g[1]=g[2]=1
for i in range(1,n):
    f=[0,0,0]
    f[0]=max(g[0],g[1])#不砍第i棵树
    if x[i-1]+h[i-1]<x[i]:#第i-1棵树向右砍是否会碰到第i棵树
        if f[0]<g[2]:
            f[0]=g[2]
    if x[i-1]<x[i]-h[i]:#向左砍第i棵树
        f[1]=max(g[0],g[1])+1
        if x[i-1]+h[i-1]<x[i]-h[i]:
            if f[1]<g[2]+1:
                f[1]=g[2]+1
    if x[i]+h[i]<x[i+1]:#向右砍第i棵树，首先要合法，即不会碰到第i+1棵树
        f[2]=max(g[0],g[1])+1
        if x[i-1]+h[i-1]<x[i]:
            if f[2]<g[2]+1:
                f[2]=g[2]+1
    g=f
print(max(g[0],g[1],g[2]))#开始n=1时的初始值在g里面不在f里
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/dwyd8wtj.png)



### 01328: Radar Installation

greedy, http://cs101.openjudge.cn/practice/01328/

思路：

错误思路：按照岛屿x坐标递增排序，从左到右分配雷达，如果当前岛屿不能被当前雷达覆盖，则新建一个，将它放在能覆盖它的最右边位置。可能会出现当前某个雷达覆盖不到较高位置的岛屿$i$，但能覆盖$i+1$，新建的雷达却覆盖不到$i+1$的情况。

区间选点问题，需要把岛屿转化为x轴上距离为d以内的区间，选取最少的点使得每一个区间内至少有一个点。

以下是照题解写的。

代码：

```python
import math
n,d=map(int,input().split())
a=[]
b=[]
cas=1
while n or d:
    a.clear()
    b.clear()
    f=1
    if d<0:
        f=0
    for _ in range(0,n):
        a.append(tuple(map(int,input().split())))
    for o in a:
        if o[1]>d:
            f=0
            break
    if f:
        for o in a:
            g=math.sqrt(d*d-o[1]*o[1])
            b.append((o[0]-g,o[0]+g))
        b.sort(key=lambda x:x[1])
        ans=1
        R=b[0][1]
        for l,r in b[1:]:
            if l>R:
                R=r
                ans+=1
        print('Case %d: %d'%(cas,ans))
    else:
        print('Case %d: -1'%(cas))
    cas+=1
    input()
    n,d=map(int,input().split())

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/h6g8eu1w.png)



## 2. 学习总结和收获

保证完成作业，巩固已学，积累做题经验和Python语法点。贪心思路需要初步判断是否正确，试着设想反例。记得把调试行删除。

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>






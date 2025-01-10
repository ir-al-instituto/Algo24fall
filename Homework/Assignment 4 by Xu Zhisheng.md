# Assignment #4: T-primes + 贪心

Updated 1715 GMT+8 Oct 16, 2024

2024 fall, Complied by <mark>徐至晟，光华</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 34B. Sale

greedy, sorting, 900, https://codeforces.com/problemset/problem/34/B



思路：

答案为前$m$小的$a_i$中负数之和的绝对值。注意：先判断$a_i$是否为负数，再计入答案。

代码

```python
L=input().split(' ')
n=int(L[0])
m=int(L[1])
a=input().split(' ')
for i in range(0,n):
    a[i]=int(a[i])
a.sort()
ans=0
for i in range(0,m):
    if (a[i]>=0):
        break
    ans-=a[i]
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/8gmdm6vf.png)



### 160A. Twins

greedy, sortings, 900, https://codeforces.com/problemset/problem/160/A

思路：

从大到小抓硬币，直到目前面值严格大于所有硬币面值的一半。

代码

```python
n=int(input())
a=input().split()
b=[int(i) for i in a]
b.sort(reverse=True)
tot=ans=cnt=0
for i in b:
    tot+=i
for i in b:
    if (ans<<1)>tot:
        break
    ans+=i
    cnt+=1
print(cnt)
```



代码运行截图 ==（至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/xpmkyta6.png)



### 1879B. Chips on the Board

constructive algorithms, greedy, 900, https://codeforces.com/problemset/problem/1879/B

思路：

贪心：最优解是每一行或每一列都有一格放置了Chip，两种情况中的较小者。

解释：若前$n-1$列都有一格放置Chip，第$n$列不放置，则至多有$n-1$行放置了Chip，使得一个$(n-1)\times (n-1)$的“子式”满足要求。剩下一行还不满足，需要在该行放置一格，此时就变为每一行都有一格放置了Chip。

代码

```python
t=int(input())
while t:
    n=int(input())
    a=input().split()
    s1=s2=0
    m1=m2=1e18
    for i in a:
        x=int(i)
        s1+=x
        if m1>x:
            m1=x
    b=input().split()
    for i in b:
        x=int(i)
        s2+=x
        if m2>x:
            m2=x
    print(min(s1+n*m2,s2+n*m1))
    t-=1
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/m9avm3lk.png)



### 158B. Taxi

*special problem, greedy, implementation, 1100, https://codeforces.com/problemset/problem/158/B

思路：

$4=4; 3+1=4; 2+2=4$

剩下的一人组和0或1个两人组，尽可能多地组成四人车。答案加上这部分人数除以4向上取整。

发现位运算的优先级小于加减，记得打括号。

代码

```python
n=int(input())
a=input().split()
cnt=[0,0,0,0,0]
for i in a:
    cnt[int(i)]+=1
ans=cnt[4]+cnt[3]+(cnt[2]>>1)
cnt[1]=max(cnt[1]-cnt[3],0)
cnt[2]&=1
ans+=((cnt[1]+(cnt[2]<<1)-1)>>2)+1
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/mcalxuln.png)



### *230B. T-primes（选做）

binary search, implementation, math, number theory, 1300, http://codeforces.com/problemset/problem/230/B

思路：

依题意得，T-primes为质数的平方。

欧拉筛的思想是每个合数都只被最小的质因子筛一遍，体现在break语句。注意欧拉筛的写法，无论当前$i$是不是质数，都要用$i\times prime_j$向后筛。

代码

```python
import math

isprime=[True]*(1000011)
tot=0

def init():
    global isprime
    global tot
    prime=[]
    isprime[1]=False
    for i in range(2,1000003):
        if isprime[i]:
            prime.append(i)
        for j in prime:
            if (i*j>1000002):
                break
            isprime[i*j]=False
            if i%j==0:
                break

n=int(input())
a=input().split()
init()
for i in a:
    x=int(math.sqrt(int(i)))
    if x*x==int(i):
        if isprime[x]:
            print('YES')
        else:
            print('NO')
    else:
        print('NO')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/dv1lazp7.png)



### *12559: 最大最小整数 （选做）

greedy, strings, sortings, http://cs101.openjudge.cn/practice/12559

思路：



代码

```python


```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

* 学会定义函数，全局变量要在函数中用global声明。
* 复习数论，欧拉筛，质数相关题目特判1。
* 通过训练培养认真审题、检查代码的品格。

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>






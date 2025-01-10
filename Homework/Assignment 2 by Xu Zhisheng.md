# Assignment #2: 语法练习

Updated 1730 GMT+8 Sep 28, 2024

2024 fall, Complied by ==徐至晟 光华==



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）课程网站是Canvas平台, https://pku.instructure.com, 学校通知9月19日导入选课名单后启用。**作业写好后，保留在自己手中，待9月20日提交。**

提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。

*注：前三道CF题目提交结果图见427A后*

## 1. 题目

### 263A. Beautiful Matrix

https://codeforces.com/problemset/problem/263/A



思路：

求1的坐标$(x,y)$到$(3,3)$的曼哈顿距离，即$|x-3|+|y-3|$

##### 代码

```python
for i in range(1,6):
    L=input().split()
    for j in range(0,5):
        if int(L[j])==1:
            x=i
            y=j+1
print(int(abs(x-3)+abs(y-3)))
```

### 1328A. Divisibility Problem

https://codeforces.com/problemset/problem/1328/A


思路：

题意是找到不小于a的最小的b的倍数。

##### 代码

```python
t=int(input())
while t:
    a, b=map(int,input().split())
    print(((a-1)//b+1)*b-a)
    t-=1
```

### 427A. Police Recruits

https://codeforces.com/problemset/problem/427/A


思路：

implementation

##### 代码

```python
n=int(input())
a=list(map(int,input().split()))
Sum=ans=0
for x in a:
    if x<0:
        if Sum>=-x:
            Sum+=x
        else:
            ans+=1
    else:
        Sum+=x
print(ans)
```



**以上三题**代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/8emvib4b.png)



### 02808: 校门外的树

http://cs101.openjudge.cn/practice/02808/



思路：

implementation

用标记数组，这一格有树记为1，每次移走一段区域的树，将这些位置修改为0。时间复杂度最坏为$O(L^2M)$，但本题区域总长度较小，可以通过。

##### 代码

```python
e=input().split()
l=int(e[0])
m=int(e[1])
a=[1]*(l+1)
for ij in range(0,m):
    e = input().split()
    for i in range(int(e[0]),int(e[1])+1):
        a[i]=0
ans=0
for i in range(0,l+1):
    ans+=a[i]
print(ans)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/ibhf0j7u.png)



### sy60: 水仙花数II

https://sunnywhy.com/sfbj/3/1/60



思路：

枚举

##### 代码

```python
a,b=map(int,input().split())
f=0
for i in range(a,b+1):
    if (i//100)**3+(i%10)**3+(i//10%10)**3==i:
        if f:
           print('',i,end='')
        else:
            print(i,end='')
            f=1
if not f:
    print('NO')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/gm0fja26.png)



### 01922: Ride to School

http://cs101.openjudge.cn/practice/01922/



思路：

此题有难度，不能直接模拟跟车的过程，应适当转化，考虑最终跟上的车是哪一辆。

出发时间为负的车子，1）如果速度不大于查理跟的车，查理不会换车；2）速度大于查理的车，不可能跟上。其余车子中，“因为最早到达的人，一定会赶上查理，而查理就会跟上他一起到达”（来源：https://www.cnblogs.com/chenxiwenruo/p/3321194.html）

所以答案为出发时间非负，最快到达Yanyuan的车的到达时间。

##### 代码

```python
import math
while (n:=int(input()))!=0:
    T=1e20
    for i in range(1,n+1):
        v,t=map(int,input().split())
        if t>=0:
            T=min(T,t+4.5/v*3600)
    print(math.ceil(T))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/0bzzgrhx.png)



## 2. 学习总结和收获

大学阶段，即使代码实现简单的题目，还是有思维含量的，正解通常比较简洁。还需打磨思维品质。

列表切片自定义排序：eg：对一个元素为元组的列表，在下标2~n-1的范围按第二关键字排序

```python
a[2:n]=sorted(a[2:n],key=lambda x:x[1])
```

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

### 每日选做：

### CF122A. Lucky Division

思路：

注意审题，仅由4、7组成的数**及其倍数**都算Almost Lucky。

$n \leq 1000$，打表所有1000以内仅含4、7的数，考虑$n$是否为它们的倍数。
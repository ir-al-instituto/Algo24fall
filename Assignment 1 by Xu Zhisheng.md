# Assignment #1: 自主学习

Updated 1750 GMT+8 Sep 12, 2024

2024 fall, Complied by ==徐至晟 光华==

## 1. 题目

### 02733: 判断闰年

http://cs101.openjudge.cn/practice/02733/

<5min

思路：

if-elif-else 语句分类讨论即可。

##### 代码

```python
a=int(input())
if a%400==0:
    print('Y')
elif a%100==0:
    print('N')
elif a%4==0:
    print('Y')
else:
    print('N')
```



代码运行截图 ==（至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/kgj6m2f6.png)



### 02750: 鸡兔同笼

http://cs101.openjudge.cn/practice/02750/

<5min

思路：

简单题。注意“至少、至多”动物的顺序

##### 代码

```python
a=int(input())
if a%2!=0:
    print('0 0')
else:
    print((a+2)//4,a//2)
```



代码运行截图 ==（至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/wxz1yu0a.png)



### 50A. Domino piling

greedy, math, 800, http://codeforces.com/problemset/problem/50/A

<8min

思路：

如果格子数是偶数，1×2的多米诺可以铺满。如果是奇数，其中一行有一格没填上，剩余行构成偶数格的情况。故答案为总格子数整除2。

##### 代码

```python
l=input().split()
m=int(l[0])
n=int(l[1])
print(m*n//2,end='')
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/xlnwszwo.png)



### 1A. Theatre Square

math, 1000, https://codeforces.com/problemset/problem/1/A

<6min

思路：

贴边排满瓷砖

##### 代码

```python
l=input().split()
n=int(l[0])
m=int(l[1])
a=int(l[2])
print(((n+a-1)//a)*((m+a-1)//a))
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/xg7mvzjf.png)



### 112A. Petya and Strings

implementation, strings, 1000, http://codeforces.com/problemset/problem/112/A

18min，包含查询语法

思路：

学会使用字符串函数

`str.upper()`和`str.lower()`分别将字符串所有字母改为大写/小写；`str.title()`首字母大写。

字符串可以按字典序直接比较大小

##### 代码

```python
A=input().lower()
B=input().lower()
if A<B:
    print(-1)
elif A==B:
    print(0)
else:
    print(1)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/15i1tic6.png)



### 231A. Team

bruteforce, greedy, 800, http://codeforces.com/problemset/problem/231/A

15min，包含查询语法

思路：

学会用`for`循环直接提取列表中的元素，这一点与C/C++不同。数每一行1的个数，如果大于2则该题目可被解决，答案加1。

##### 代码

```python
n=int(input())
ans=0
for i in range(0,n):
    Sum=0
    l=input().split()
    for j in l:
        Sum+=int(j)
    if Sum>=2:
        ans+=1
print(ans)
```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/085862z0.png)



## 2. 学习总结和收获

前信竞省二选手，算法大量遗忘，C++转型Python中。

前四题较简单，后两题帮助了解字符串、列表、for语句的语法。

### ==额外练习题目==

每日选做：

Sep 10 课前完成：

* 25A IQ Test
* 34B Sale
* 96A Football
* 1374B Multiply by 2, divide by 6
* 1475A Odd Divisor

Sep 10 Evening:

* 1833B Restore the Weather

### 1833B. Restore the Weather

两天前尝试失败，总用时1h+

涉及语法比较多，有列表、元组/字典、排序用 lambda 函数选择关键字等。

写之前看过题解。贪心，把预报气温和真实气温均从小到大排序，一一对应。还需要把气温列表还原为预报输入的顺序。

贪心思路正确性可以邻项交换证明。

假设`a,b`数组排好序了，`a[i]`对应`b[i]`，`a[i+1]`对应`b[i+1]`

如果`a[i]`对应`b[i+1]`，`a[i+1]`对应`b[i]`，则两者之差绝对值中，至少有一个增大，可能就不满足要求了。

##### 代码

```python
t=int(input())
while t>0:
    li=input().split()
    n=int(li[0])
    k=int(li[1])
    a=input().split()
    for i in range(0,n):
        a[i]=(int(a[i]),i)
    a.sort(key=lambda x:x[0])
    b=input().split()
    b=[int(i) for i in b]
    b.sort()
    c=[]
    for i in range(0,n):
        c.append((a[i][1],b[i]))
    c.sort(key=lambda x:x[0])
    print(c[0][1],end='')
    for i in range(1,n):
        print('',c[i][1],end='')
    print('')
    t-=1
```

代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/cr5fw4om.png)
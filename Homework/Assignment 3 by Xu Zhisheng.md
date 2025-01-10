# Assign #3: Oct Mock Exam暨选做题目满百

2024 fall, Complied by 徐至晟，光华


**说明：**

1）Oct⽉考： ***AC=4***。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++/C（已经在Codeforces/Openjudge上AC），截图（包含Accepted, 学号），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、作业评论有md或者doc。

4）如果不能在截止前提交作业，请写明原因。

### 原因

没有关注该项作业，最后一天晚上才知道，且因为高数习题课和学院会议，23:00之后才使用电脑整理，故未及时完成。

==*注：1-4题AC截图见“角谷猜想”末尾处*==

## 1. 题目

### E28674:《黑神话：悟空》之加密

http://cs101.openjudge.cn/practice/28674/

思路：

本题知识点为：chr() 与 ord() 函数实现ASCII码与字符之间转换；
ch.isupper(), islower(), isdigit() 函数判断字符是否是大小写字母、数字

代码

```python
k=int(input())
s=input()
for i in s:
    if i.islower():
        print(chr((ord(i)-97-k)%26+97),end='')
    elif i.isupper():
        print(chr((ord(i)-65-k)%26+65),end='')
```

### E28691: 字符串中的整数求和

http://cs101.openjudge.cn/practice/28691/



思路：

直接在特定下标提取数字

代码

```python
s=input()
print((ord(s[0])-48)*10+ord(s[1])-48+(ord(s[4])-48)*10+ord(s[5])-48)
```

### M28664: 验证身份证号

http://cs101.openjudge.cn/practice/28664/



思路：

模拟+按题意打表

代码

```python
n=int(input())
k=[7,9,10,5,8,4,2,1,6,3,7,9,10,5,8,4,2]
numero=[1,0,10,9,8,7,6,5,4,3,2]
while n:
    s=input()
    tot=0
    for i in range(0,17):
        tot=(tot+(ord(s[i])-48)*k[i])%11
    if s[17]=='X':
        x=10
    else:
        x=ord(s[17])-48
    if numero[tot]==x:
        print('YES')
    else:
        print('NO')
    n-=1
```

### M28678: 角谷猜想

http://cs101.openjudge.cn/practice/28678/



思路：

简单模拟

代码

```python
n=int(input())
while n!=1:
    if n&1:
        print(n,'*3+1=',n:=n*3+1,sep='')
    else:
        print(n,'/2=',n:=n>>1,sep='')
print('End')
```



**1-4题**代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/56udajah.png)



### M28700: 罗马数字与整数的转换

http://cs101.openjudge.cn/practice/28700/

==***本题未AC***==

思路：

本题比较复杂，考试时没有想到合适的方法，陷入思维困局。

对于阿拉伯数字转罗马数字，应该按1000,500,400,...,10,5,4,1的顺序递降，依次减掉，然后加上相应的字母。考场迷思在于如何对4进行特判，但没有必要。

对于罗马数字转阿拉伯数字，题解是逐个字母从前向后读取的，设置了prev_value变量记录上一个字母对应的值，如果出现前小后大额外特判。

##### 代码

```python
# 

```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





### *T25353: 排队 （选做）

http://cs101.openjudge.cn/practice/25353/

==***本题未AC***==

思路：

~~难度已超标~~

代码

```python


```



代码运行截图 ==（AC代码截图，至少包含有"Accepted"）==





## 2. 学习总结和收获

没有掌握字典，导致第五题无法解出。第六题本身超过能力范围。可能是前期训练不够全面，没有解决所有Python语法点，不太能接受某些灵活的表达，如for语句的引用方式；同时仍受中学竞赛时某些僵化思维的影响。

==如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。==

Qué será, será. What will be, will be.








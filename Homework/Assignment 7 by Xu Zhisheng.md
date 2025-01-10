# Assignment #7: Nov Mock Exam立冬

Updated GMT+8 Nov 7, 2024

2024 fall, Complied by 徐至晟，光华



**说明：**

1）⽉考： <mark>**AC6**</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。

## 0.考试提交记录截图（AC6）

![](https://cdn.luogu.com.cn/upload/image_hosting/49pi0lbu.png)

## 1. 题目

### E07618: 病人排队

sorting, http://cs101.openjudge.cn/practice/07618/

思路：

把老人和年轻人分开；老人按年龄和排队顺序记录关键字。

因为年龄是从大到小排序，排队序号是从小到大，如果不写比较标准，放在一个元组里，需要给其中一个加上负号，使之标准统一，能一起排序。

```a.sort(reverse=True)```从大到小排序

代码：

```python
n=int(input())
mayores=[]
jovenes=[]
for i in range(0,n):
    st=input().split()
    x=st[0]
    edad=int(st[1])
    if edad>=60:
        mayores.append((edad,-i,x))
    else:
        jovenes.append(x)
mayores.sort(reverse=True)
for p in mayores:
    print(p[2])
for p in jovenes:
    print(p)
```



### E23555: 节省存储的矩阵乘法

implementation, matrices, http://cs101.openjudge.cn/practice/23555/

思路：

Python中二维数组的存储与我想象的不同，出现错乱。改用C++完成。本题无需过多优化，直接翻译题面意思，把两个矩阵用二维数组存储也不会超空间。*不过建议写明矩阵维数 n 的数据范围*

代码：

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<algorithm>
#include<cstdlib>
#include<cmath>
#include<map>
using namespace std;
const int N=505;
int n,m1,m2,a[N][N],b[N][N];
int main(){
	scanf("%d%d%d",&n,&m1,&m2);
	int x,y,z;
	for(int i=1;i<=m1;++i)
	{
		scanf("%d%d%d",&x,&y,&z);
		a[x][y]=z;
	}
	for(int i=1;i<=m2;++i)
	{
		scanf("%d%d%d",&x,&y,&z);
		b[x][y]=z;
	}
	for(int i=0;i<n;++i)
		for(int j=0;j<n;++j)
		{
			z=0;
			for(int k=0;k<n;++k)
				z+=a[i][k]*b[k][j];
			if (z) {printf("%d %d %d\n",i,j,z);}
		}
	return 0;
}
```



### M18182: 打怪兽 

implementation/sortings/data structures, http://cs101.openjudge.cn/practice/18182/

思路：

按技能发动时间为第一关键字排序，若时间相同则优先选择伤害高的，排在前面。遍历时，若前后技能发动时间相同，需要计数已使用的技能，即选择前m大的。时间复杂度$\mathrm{\Theta}(n \log n)$。

代码：（头文件、namespace略去）

```cpp
int T,n,m,b;
struct node{
	int t,x;
}a[1003];
bool cmp(node X,node Y){return X.t<Y.t || (X.t==Y.t && X.x>Y.x);}
int main(){
	scanf("%d",&T);
	while(T--)
	{
		scanf("%d%d%d",&n,&m,&b);
		for(int i=1;i<=n;++i)
			scanf("%d%d",&a[i].t,&a[i].x);
		sort(a+1,a+1+n,cmp);
		int cnt=0;
		for(int i=1;i<=n;++i)
		{
			if (a[i].t!=a[i-1].t) cnt=m;
			if (cnt) b-=a[i].x,cnt-=1;
			if (b<=0){printf("%d\n",a[i].t);break;}
		}
		if (b>0) printf("alive\n");
	}
	return 0;
}
```



### M28780: 零钱兑换3

dp, http://cs101.openjudge.cn/practice/28780/

思路：

完全背包DP最优化。

代码（略去头文件）：

```cpp
int n,m,f[1000003];
int main(){
	memset(f,0x7f,sizeof(f));
	f[0]=0;
	scanf("%d%d",&n,&m);
	for(int i=1,v;i<=n;++i)
	{
		scanf("%d",&v);
		for(int k=v;k<=m;++k)
			if (f[k]>f[k-v]+1)
				f[k]=f[k-v]+1;
	}
	if (f[m]>10000000) printf("-1");
	else printf("%d\n",f[m]);
	return 0;
}
```



### T12757: 阿尔法星人翻译官

implementation, http://cs101.openjudge.cn/practice/12757

思路：

递归

* 数字的英文表达具有二叉树中序遍历的结构，Million, thousand, hundred为从上到下的各级父亲节点（以下称为级词）。

* 递归的边界（二叉树的叶子节点）是没有级词的子串（即99以内的数），所有单词对应的数字直接相加。
* 以Million为例，xxx million yyy：$x\times 10^6 + y$，其中$x,y$分别是xxx, yyy代表的数字，由下一级递归返回。

代码：（考试源码基础上添加注释）

```python
d={'zero':0, 'one':1, 'two':2, 'three':3, 'four':4, 'five':5,
    'six':6, 'seven':7, 'eight':8, 'nine':9, 'ten':10,
    'eleven':11, 'twelve':12, 'thirteen':13, 'fourteen':14, 'fifteen':15,
    'sixteen':16, 'seventeen':17, 'eighteen':18, 'nineteen':19,
    'twenty':20, 'thirty':30, 'forty':40, 'fifty':50,
    'sixty':60, 'seventy':70, 'eighty':80, 'ninety':90, 'hundred':100,
    'thousand':1000, 'million':1000000}

s=input().split()

def buscar(l,r,a):
    #寻找当前区间内是否有“级词”，如果没有返回-1，降级
    global s
    for i in range(l,r):
        if a==-1 and s[i]=='million':
            return i
        elif a==-2 and s[i]=='thousand':
            return i
        elif a==-3 and s[i]=='hundred':
            return i
    return -1
def numero(a):
    #“级词”对应的倍数
    if a==-1:
        return 1000000
    if a==-2:
        return 1000
    if a==-3:
        return 100
def trabajo(l,r,a):
    global s,d
    if l>=r:
        return 0
    if a<-3:
        Num=0
        for i in range(l,r):
            Num+=d[s[i]]
        return Num
    k=buscar(l,r,a)
    if k<0:
        return trabajo(l,r,a-1)
    else:
        return trabajo(l,k,a-1)*numero(a)+trabajo(k+1,r,a-1)

if s[0]=='negative':
    print(-trabajo(1,len(s),-1))
else:
    print(trabajo(0,len(s),-1))
```



### T16528: 充实的寒假生活

greedy/dp, cs10117 Final Exam, http://cs101.openjudge.cn/practice/16528/

思路：

DP求最多不重叠区间。按右端点从小到大排序，$f_i$表示选择第$i$个活动时前$i$个活动能参加的最大数目。

$f_i=1 &\quad\mathrm{default} \\ f_i=\max_{0<j<i}\{f_j\}+1 \quad &\mathrm{if}\; r_j<l_i$

代码（略去头文件）：

```cpp
int n,ans,f[10003];
struct node{
	int l,r;
}a[10003];
bool cmp(node x,node y){
	return x.r<y.r;
}
int main(){
	scanf("%d",&n);
	for(int i=1;i<=n;++i)
		scanf("%d%d",&a[i].l,&a[i].r);
	sort(a+1,a+1+n,cmp);
	for(int i=1,m;i<=n;++i)
	{
		m=0;
		for(int j=1;j<i;++j)
			if (a[j].r<a[i].l)
				m=max(m,f[j]);
		f[i]=m+1;
		ans=max(ans,f[i]);
	}
	printf("%d\n",ans);
	return 0;
}
```



## 2. 学习总结和收获

本次月考两道DP是模板题，与作业类似。对Python的字典以及二维数组的存储规则不熟悉，使用C++完成了部分题目。T5 implementation 吸取了上次月考的教训，通过递归和打表成功求解。和1小时内AK的同学仍然存在差距。

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>






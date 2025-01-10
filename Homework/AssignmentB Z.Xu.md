# Assignment #B: Dec Mock Exam大雪前一天

Updated Dec 6, 2024

2024 fall, Complied by 徐至晟，光华



**说明：**

1）⽉考： <mark>**AC2 ** T_T</mark> 。考试题⽬都在“题库（包括计概、数算题目）”⾥⾯，按照数字题号能找到，可以重新提交。作业中提交⾃⼰最满意版本的代码和截图。

2）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

3）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

4）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### E22548: 机智的股民老张

http://cs101.openjudge.cn/practice/22548/

思路：（考场AC）

记录目前假设的买入价格，如果遇到更低的价格修改买入价，更高则尝试更新收入。

代码：

```python
a=list(map(int,input().split()))
compro=10001
dinero=0
for i in a:
    if i>compro:
        dinero=max(dinero,i-compro)
    else:
        compro=i
print(dinero)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### M28701: 炸鸡排

greedy, http://cs101.openjudge.cn/practice/28701/

思路：

考场被时间线所迷惑，没有想到把总时间对k口锅平均，同之前作业“万柳骑车”的思维困境类似。

考后参考题解：

[[木桶差异\] 炸鸡排_炸鸡排csdn-CSDN博客](https://blog.csdn.net/StudyingPanda/article/details/135091917)

[IntroductionToComputationHomework2022/期末考试/炸鸡排.cpp at master · LC-John/IntroductionToComputationHomework2022 · GitHub](https://github.com/LC-John/IntroductionToComputationHomework2022/blob/master/期末考试/炸鸡排.cpp)

* 每次放上锅炸鸡再取下的时间$t\in \mathbb{R}$
* 耗时较长的鸡排视为占一口锅无法取下（考场想到这一点，不知如何转化）因此不断去除当前集合中超出“平均时间”的鸡排，锅的数量减一
* 对比两篇题解，第二篇用排序从大到小删除较自然，第一篇不排序容易引起误导，遍历一遍不够需要反复删除

代码：

```python
n,k=map(int,input().split())
t=list(map(int,input().split()))
s=0.0
for a in t:
    s+=a
t.sort(reverse=True)
for a in t:
    if a>s/k:
        s-=a
        k-=1
print("%.3f"%(s/k))
```



代码运行截图 ==（至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/dh4c8rxz.png)



### M20744: 土豪购物

dp, http://cs101.openjudge.cn/practice/20744/

思路：

考场尝试了$\mathrm{\Theta}(n^3)$的brute force枚举、$\mathrm{\Theta}(n^2)$选删除的数然后向两边找最大的区间和，均超时。

最后赌一把贪心，选择负数绝对值最大的删除，错误。

考场$\mathrm{\Theta}(n^2)$核心代码如下：

```python
for i in a:
    ans=max(ans,i)
    s.append(s[-1]+i)
for k in range(0,n):
    rm=-Inf
    lm=-Inf
    for i in range(k+1,n):
        rm=max(rm,s[i+1]-s[k+1])
    for i in range(k-1,-1,-1):
        lm=max(lm,s[k]-s[i])
    g=max(lm+rm,lm,rm)
    if g>=0:
        ans=max(ans,g,g+a[k])
print(ans)
```

可以发现固定$k$，$s[k],s[k+1]$是常数，因此只要找$s[i],s[i+1]$的最大值，~~若用树状数组或线段树维护，时间复杂度为$\mathrm{\Theta}(n\log n)$，应该可以通过。~~

考完竟然注意到只需维护前缀和数组的“前缀最大值”和“后缀最大值”！因此可以$\mathrm{\Theta}(n)$预处理，每次$\mathrm{\Theta}(1)$查找，实现线性复杂度求解。

代码：

```python
Inf=int(1e18)
a=[0]+list(map(int,input().split(',')))
n=len(a)-1
s=[0]
slmin=[0]
srmax=[-Inf for _ in range(n+2)]
ans=-Inf
for i in range(1,n+1):
    ans=max(ans,a[i])
    s.append(s[i-1]+a[i])
    slmin.append(min(s[-1],slmin[-1]))
if ans>0:
    srmax[n]=s[n]
    for i in range(n-1,0,-1):
        srmax[i]=max(srmax[i+1],s[i])
    for k in range(1,n+1):
        rm=srmax[k+1]-s[k]
        lm=s[k-1]-slmin[k-1]
        g=max(lm+rm,lm,rm)
        if g>=0:
            ans=max(ans,g,g+a[k])
print(ans)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/xnjb72gq.png)



### T25561: 2022决战双十一

brute force, dfs, http://cs101.openjudge.cn/practice/25561/

思路：

最奇葩输入，注意区分商品和店铺；思路很brute force，搜索回溯，枚举每一件商品在哪一家店购买，统计每一家店消费的总标价；看清楚消费券的规则。

代码：

```python
n,m=map(int,input().split())
cosa=[[-1 for _ in range(m+1)] for _ in range(n+1)]
cupon=[[] for _ in range(m+1)]
for i in range(1,n+1):
    st=input().split()
    for j in st:
        tienda,precio=map(int,j.split(':'))
        cosa[i][tienda]=precio
for i in range(1,m+1):
    st=input().split()
    for j in st:
        cupon[i].append(tuple(map(int,j.split('-'))))
cuesta=[0 for _ in range(m+1)]
res=int(1e18)
ahora=0
def dfs(x):
    global n,m,res,ahora,cosa,cupon,cuesta
    if x>n:
        final=ahora-(ahora//300)*50
        for tienda in range(1,m+1):
            mejor=0
            for c in cupon[tienda]:
                if cuesta[tienda]>=c[0]:
                    mejor=max(mejor,c[1])
            final-=mejor
        res=min(res,final)
        return
    for tienda in range(1,m+1):
        if cosa[x][tienda]>=0:
            cuesta[tienda]+=cosa[x][tienda]
            ahora+=cosa[x][tienda]
            dfs(x+1)
            ahora-=cosa[x][tienda]
            cuesta[tienda]-=cosa[x][tienda]
dfs(1)
print(res)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/rzlw05i1.png)



### T20741: 两座孤岛最短距离

dfs, bfs, http://cs101.openjudge.cn/practice/20741/

思路：（考场AC）

第一次提交超时思路：洪水填充找出两座岛，然后求两个点集各取一点最近的距离。点集大小乘积不超过$(\frac{1}{2}n^2)^2$，最坏时间复杂度为$\mathrm{\Theta}(n^4)$。

改进$\mathrm{\Theta}(n^2)$：只找一座岛，把它的所有点作为BFS的起点走地图，最先走到另一座岛即为搭桥需要的最少1数目。

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
const int dx[4]={1,0,-1,0},dy[4]={0,1,0,-1};
int n,c,v[303][303],d[303][303];
char s[303][303];
queue<pair<int,int> > q;
void dfs(int x,int y){
	if (s[x][y]!='1' or v[x][y])
        return;
    v[x][y]=c;
    q.push(make_pair(x,y));
    if (x) dfs(x-1,y);
    if (y) dfs(x,y-1);
    if (x<n-1) dfs(x+1,y);
    if (y<n-1) dfs(x,y+1);
}
int main(){
	scanf("%d",&n);
	for(int i=0;i<n;++i)
		scanf("%s",s[i]);
	for(int i=0;i<n;++i)
 		for(int j=0;j<n;++j)
  	    	if (v[i][j]==0 and s[i][j]=='1'){
				c+=1;
            if (c>1) break;
            dfs(i,j);}
    int x,y,g,h,fl=0;
    while(!q.empty()){
    	x=q.front().first,y=q.front().second;q.pop();
    	for(int i=0;i<4;++i){
    		g=x+dx[i],h=y+dy[i];
    		if (g>=0 and g<n and h>=0 and h<n)
    			if (v[g][h]==0)
    			{
    				d[g][h]=d[x][y]+1;
    				v[g][h]=1;q.push(make_pair(g,h));
    				if (s[g][h]=='1'){fl=1;break;}
				}
		}
		if (fl) break;
	}
	printf("%d",d[g][h]-1);
	return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/mmbbxszr.png)



### T28776: 国王游戏

greedy, http://cs101.openjudge.cn/practice/28776

思路：

NOIP2012提高组

曾经洛谷紫题（现在降为绿题），变成背结论价值不大。考察邻项交换证明贪心策略之排序方式。

审题！

***每位大臣获得的金币数分别是：排在该大臣<mark>前面</mark>的所有人的左手上的数的乘积除以他自己右手上的数，然后向下取整得到的结果***

记得排序标准是两手数字乘积，但忘记了国王也在队列的开头，所以有n+1行输入两个数！考场只输入n组，发现输出错误怀疑背的结论是错的，于是弃疗，心态亦不佳矣！

代码：

```python
n=int(input())
num_real=tuple(map(int,input().split()))
h=[]
for _ in range(n):
    a,b=map(int,input().split())
    h.append((a*b,a,b))
h.sort()
m=num_real[0]
elmas=1
for grupo in h:
    elmas=max(elmas,m//grupo[2])
    m*=grupo[1]
print(elmas)
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/7esh9uc3.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

1.本次测试，有几道题没有数据范围，影响对题目要求的判断，且困扰C/C++开数组，希望能提供。

2.合理安排开题顺序

3.审题

4.“土豪购物”败在临门一脚，熟练度、思维需提升


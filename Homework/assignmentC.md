# Assignment #C: 五味杂陈 

Updated 1148 GMT+8 Dec 10, 2024

2024 fall, Complied by <mark>同学的姓名、院系</mark>



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 1115. 取石子游戏

dfs, https://www.acwing.com/problem/content/description/1117/

思路：

按题目提示递归，函数返回值为布尔变量，取一次石子先后手交换，返回相反的结果。注意边界条件，如果有两堆一样的则“当前先手”胜。

代码：

```python
def f(x,y):
    if x<y:
        return f(y,x)
    if x>=y*2:
        return True
    elif x==y:
        return True
    else:
        return not f(x-y,y)

a,b=map(int,input().split())
while a:
    if f(a,b):
        print("win")
    else:
        print("lose")
    a,b=map(int,input().split())
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/u5g0y4c4.png)



### 25570: 洋葱

Matrices, http://cs101.openjudge.cn/practice/25570

思路：

implementation

代码：

```python
n=int(input())
a=[]
for _ in range(n):
    b=list(map(int,input().split()))
    a.append(b)
ans=0
for i in range(0,(n+1)//2):
    s=0
    r=n-i-1
    if i==r:
        ans=max(ans,a[i][i])
        break
    for j in range(i,r+1):
        s+=a[i][j]+a[j][i]+a[r][j]+a[j][r]
    s-=a[i][i]+a[i][r]+a[r][i]+a[r][r]
    ans=max(ans,s)
print(ans)
```



代码运行截图 ==（至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/bm1vcf57.png)



### 1526C1. Potions(Easy Version)

greedy, dp, data structures, brute force, *1500, https://codeforces.com/problemset/problem/1526/C1

思路：

无思路，参考题解，反悔贪心。用优先队列存储Δhp为负数的瓶子；先喝下每一瓶，如果hp变成负数，删除之前最掉hp的一瓶，保证hp变回非负。（代码中while改写成if也成立）

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
ll n,hp,cnt;
priority_queue<ll> q;
int main()
{
	scanf("%lld",&n);
	for(ll i=1,a;i<=n;++i)
	{
		scanf("%lld",&a);
		hp+=a;cnt+=1;
		if (a<0) q.push(-a);
		while(!q.empty()&&hp<0)
			hp+=q.top(),cnt-=1,q.pop();
	}
	printf("%lld",cnt);
	return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/nx2lx6y1.png)



### 22067: 快速堆猪

辅助栈，http://cs101.openjudge.cn/practice/22067/

思路：

单调栈，如果当前进入栈的元素值比前面的大，则没有贡献，将前面的最小值入栈，维护栈底到栈顶单调不增。

代码：

```python
a=[]
try:
    while True:
        s=input()
        if s[1]=='o':
            if len(a):
                a.pop()
        elif s[1]=='u':
            b=s.split()
            x=int(b[1])
            if len(a):
                a.append(min(a[-1],x))
            else:
                a.append(x)
        else:
            if len(a):
                print(a[-1])
except EOFError:
    pass
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/mbhsvejk.png)



### 20106: 走山路

Dijkstra, http://cs101.openjudge.cn/practice/20106/

思路：

C++ unAC

思路与题解相同，但采用普通队列搜索，同一个点可以出入队多次，没有用Dijkstra。开始是读入和预处理问题，后面修改了仍然无法通过。

Python优先队列：

```python
from heapq import *
heap=[]
a=1
heappush(heap,a)
a=heappop(heap)
# while heap: 等效于C++ while(!q.empty())
```

建议把输入中的#改为-1之类的数字，否则干扰读入，偏离了考察算法的核心任务。

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll dx[4]={1,0,-1,0},dy[4]={0,1,0,-1};
queue<pair<ll,ll> > q;
ll m,n,p,h[103][103],d[103][103],in[103][103];
char s[10003];
int main(){
	memset(h,0x7f,sizeof(h));
	scanf("%lld%lld%lld",&m,&n,&p);
	fgets(s,10003,stdin);
	for(ll i=1;i<=m;++i){
		fgets(s,10003,stdin);
		ll x=0,f=1,k=0;
		for(ll j=0;j<strlen(s);++j)
		{
			if (isdigit(s[j]))
				x=x*10+(s[j]-48),f=0;
			else{if (s[j]==' ') h[i][++k]=x,x=0;
			else{if (s[j]=='#') h[i][++k]=-1,x=0,f=1;}}
		}
		if (!f) h[i][++k]=x;}
	ll a,b,c,e,x,y,w,z,g;
	while(p){
		scanf("%lld%lld%lld%lld",&a,&b,&c,&e);
		a+=1,b+=1,c+=1,e+=1;
		if (h[a][b]<0 or h[c][e]<0) {printf("NO\n");continue;}
		memset(d,0x7f,sizeof(d));
		memset(in,0,sizeof(in));
		d[a][b]=0,in[a][b]=1;
		while(!q.empty()) q.pop();
		q.push(make_pair(a,b));
		while(!q.empty()){
			x=q.front().first,y=q.front().second;q.pop();
			in[x][y]=0;
			for(ll i=0;i<4;++i){
				w=x+dx[i],z=y+dy[i];
                if (w<=0 or w>m or z<=0 or z>n or h[w][z]<0) continue;
				g=abs(h[w][z]-h[x][y]);
				if (d[w][z]>d[x][y]+g){
					d[w][z]=d[x][y]+g;
					if (!in[w][z]){
						in[w][z]=1;
						q.push(make_pair(w,z));
					}
				}
			}
		}
		if (d[c][e]>1e18) printf("NO\n");
		else printf("%lld\n",d[c][e]);
		p-=1;
	}
	return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





### 04129: 变换的迷宫

bfs, http://cs101.openjudge.cn/practice/04129/

思路：

本周没有时间完成。思路与之前作业类似，visited数组需要有额外信息，即是否在k的倍数时间点到达某位置。

代码：

```python

```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>





## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

只有前两题简单，写bfs工程量较大容易心态爆炸




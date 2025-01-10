# Assignment #9: dfs, bfs, & dp

2024 fall, Complied by 徐至晟，光华

**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

###### *前三题代码运行截图*<mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/jfkat8y3.png)

### 18160: 最大连通域面积

dfs similar, http://cs101.openjudge.cn/practice/18160

思路：

洪水填充

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
int T,n,m,tot,v[1003][1003],ans[1000003];
char s[1003][1003];
void dfs(int x,int y)
{
	for(int dx=1;dx>=-1;--dx)
		for(int dy=1;dy>=-1;--dy)
			if (s[x+dx][y+dy]=='W' && !v[x+dx][y+dy])
				v[x+dx][y+dy]=tot,dfs(x+dx,y+dy); 
}
int main()
{
	scanf("%d",&T);
	while(T--)
	{
		scanf("%d%d",&n,&m);
		memset(s,0,sizeof(s));
		for(int i=1;i<=n;++i)
			scanf("%s",s[i]+1);
		memset(v,0,sizeof(v));
		memset(ans,0,sizeof(ans));
		tot=0;
		for(int i=1;i<=n;++i)
			for(int j=1;j<=m;++j)
				if (s[i][j]=='W'){
					if (!v[i][j])
						v[i][j]=++tot,dfs(i,j);
					++ans[v[i][j]];}
		for(int i=1;i<=tot;++i)
			ans[0]=max(ans[0],ans[i]);
		printf("%d\n",ans[0]);
	}
	return 0;
}
```



### 19930: 寻宝

bfs, http://cs101.openjudge.cn/practice/19930

思路：

基础BFS走地图

代码中将二维坐标映射到一维，$(x,y)\rightarrow(x-1)n+y$，向下、向上走$\pm n$。值得注意的是向右、向左走$\pm1$之前需要判断是否位于一行的开头或结尾，对列数$n$取模判断。

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
queue<int> q;
int m,n,ed,a[2503],d[2503],in[2503];
void work(int s,int u)
{
	if (u<=0 || u>=m*n) return;
	if (a[u]!=2 && d[u]>d[s]+1) {d[u]=d[s]+1;
		if (!in[u]) in[u]=1,q.push(u);}
}
int main()
{
	scanf("%d%d",&m,&n);
	for(int i=1;i<=m;++i)
		for(int j=1,lo;j<=n;++j)
		{
			lo=(i-1)*n+j;
			scanf("%d",&a[lo]);
			if (a[lo]==1) ed=lo;
		}
	memset(d,0x7f,sizeof(d));
	d[1]=0,in[1]=1,q.push(1);
	int p;
	while(!q.empty())
	{
		p=q.front();
		q.pop();in[p]=0;
		if (p==ed) break;
		if (p%n) work(p,p+1);
		work(p,p+n);
		if ((p-1)%n) work(p,p-1);
		work(p,p-n);
	}
	if (d[ed]<=m*n) printf("%d",d[ed]);
	else printf("NO");
	return 0;
}
```



### 04123: 马走日

dfs, http://cs101.openjudge.cn/practice/04123

思路：

搜索回溯。但是为什么本地超时OJ能通过？

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
int t,m,n,a,b,plan,v[10][10];
void dfs(int x,int y,int cnt)
{
	if (cnt==m*n){++plan;return;}
	for(int dx=-2,dy;dx<=2;++dx)
	{
		if (dx==0||x+dx<0||x+dx>=n) continue;
		dy=2/dx;
		if (y+dy>=0&&y+dy<m&&!v[x+dx][y+dy])
			v[x+dx][y+dy]=1,
			dfs(x+dx,y+dy,cnt+1),v[x+dx][y+dy]=0;
		dy*=-1;
		if (y+dy>=0&&y+dy<m&&!v[x+dx][y+dy])
			v[x+dx][y+dy]=1,
			dfs(x+dx,y+dy,cnt+1),v[x+dx][y+dy]=0;
	}
}
int main()
{
	scanf("%d",&t);
	while(t--)
	{
		scanf("%d%d%d%d",&n,&m,&a,&b);
		plan=0;
		v[a][b]=1,dfs(a,b,1),v[a][b]=0;
		printf("%d\n",plan);
	}
	return 0;
}
```



### sy316: 矩阵最大权值路径

dfs, https://sunnywhy.com/sfbj/8/1/316

思路：

dfs，在走地图同时顺便记录路径，更新答案时复制路径数组。

代码：

```python
#include<bits/stdc++.h>
using namespace std;
const int dx[4]={1,0,-1,0},dy[4]={0,1,0,-1};
int n,m,Max=-1e9,a[7][7],v[7][7];
int p[27][2],ans[27][2];
void dfs(int x,int y,int c,int s)
{
	if (x==n&&y==m){
		if (s>Max) {Max=s;memcpy(ans,p,sizeof(p));}
		return;
	}
	for(int i=0,g,h;i<4;++i)
	{
		g=x+dx[i],h=y+dy[i];
		if (g>0&&g<=n&&h>0&&h<=m&&!v[g][h])
		{
			p[c+1][0]=g,p[c+1][1]=h;
			v[g][h]=1;
			dfs(g,h,c+1,s+a[g][h]);
			p[c+1][0]=0,p[c+1][1]=0;
			v[g][h]=0;
		}
	}
}
int main()
{
	scanf("%d%d",&n,&m);
	for(int i=1;i<=n;++i)
		for(int j=1;j<=m;++j)
			scanf("%d",&a[i][j]);
	p[1][0]=1,p[1][1]=1,v[1][1]=1;
	dfs(1,1,1,a[1][1]);
	for(int i=1;ans[i][0];++i)
		printf("%d %d\n",ans[i][0],ans[i][1]);
	return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/qek4dr1l.png)





### LeetCode62.不同路径

dp, https://leetcode.cn/problems/unique-paths/

思路：

杨辉三角递推法计算组合数。答案为$C_{m+n-1}^{m-1}$

代码：

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        f=[[0 for _ in range(n)] for _ in range(m)]
        f[0][0]=1
        for i in range(0,m):
            for j in range(0,n):
                if i==0 or j==0:
                    f[i][j]=1
                else:
                    f[i][j]=f[i-1][j]+f[i][j-1]
        return f[m-1][n-1]
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/mnpavlw3.png)



### sy358: 受到祝福的平方

dfs, dp, https://sunnywhy.com/sfbj/8/3/539

思路：

DFS，寻找分段点，有一种方案成立即返回True。 体现Python优势的题目，字符串切片转整数

代码：

```python
import math
a=input()
n=len(a)

def dfs(x):
    global a,n
    if x==n:
        return True
    for y in range(x+1,n+1):
        b=int(a[x:y])
        if b>0 and b==(int(math.sqrt(b))**2):
            if dfs(y):
                return True
    return False

if dfs(0):
    print('Yes')
else:
    print('No')
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/sfzlzln3.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

一次AC率明显下降，各种小细节错误很多，如多组数据初始化、输入时循环上线n,m混淆等。




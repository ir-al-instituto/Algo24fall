# Assignment #D: 十全十美 

2024 fall, Complied by 徐至晟，光华



**说明：**

1）请把每个题目解题思路（可选），源码Python, 或者C++（已经在Codeforces/Openjudge上AC），截图（包含Accepted），填写到下面作业模版中（推荐使用 typora https://typoraio.cn ，或者用word）。AC 或者没有AC，都请标上每个题目大致花费时间。

2）提交时候先提交pdf文件，再把md或者doc文件上传到右侧“作业评论”。Canvas需要有同学清晰头像、提交文件有pdf、"作业评论"区有上传的md或者doc附件。

3）如果不能在截止前提交作业，请写明原因。



## 1. 题目

### 02692: 假币问题

brute force, http://cs101.openjudge.cn/practice/02692

思路：

本题思维难度中等，但做到思路清晰较难。输入时，将每次称重右侧轻、重、平分别记为-1,1,0存在b数组中，并且记录每一组左右秤盘中存在的硬币。从A到K依次枚举硬币，假设它偏重，第i组称重如果它在左侧，a[i]=1；在右侧a[i]=0。比较a与b的各个元素是否都相等，如果是则满足条件，选出了假币。假设偏轻只需要把a数组全部取相反数与b比较。

代码（C++）：

```python
#include<bits/stdc++.h>
using namespace std;
int exist[3][2][15],a[3],b[3];
int main()
{
	int n;
	char s[15],sta[7];
	scanf("%d",&n);
	while(n--){
		memset(exist,0,sizeof(exist));
		memset(b,0,sizeof(b));
		for(int i=0;i<3;++i){
			for(int j=0;j<2;++j){
				scanf("%s",s);
				for(int g=0;g<strlen(s);++g)
					exist[i][j][s[g]-64]=1;
			}
			scanf("%s",sta);
			switch(sta[0]){
				case 'u':
					b[i]=1;
					break;
				case 'd':
					b[i]=-1;
					break;
				default:
					b[i]=0;
					break;
			}
		}
		for(int i=1;i<=12;++i)
		{
			memset(a,0,sizeof(a));
			for(int j=0;j<3;++j)
			{
				if (exist[j][0][i]) a[j]=1;
				else if (exist[j][1][i]) a[j]=-1;
			}
			if (a[0]==b[0]&&a[1]==b[1]&&a[2]==b[2]){
				printf("%c is the counterfeit coin and it is heavy.\n",(char)(i+64));
				break;}
			else{if (a[0]+b[0]==0&&a[1]+b[1]==0&&a[2]+b[2]==0){
				printf("%c is the counterfeit coin and it is light.\n",(char)(i+64));
				break;}
			}
		}
	}
	return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/pkk0cbz4.png)



### 01088: 滑雪

dp, dfs similar, http://cs101.openjudge.cn/practice/01088

思路：

受群里启发，将各点高度排序，从高到低平推即可。该图是有向无环图

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
const int dx[4]={0,1,0,-1},dy[4]={1,0,-1,0};
int r,c,tot,m[103][103],d[103][103];
struct node{
	int h,x,y;
}a[10003];
bool cmp(node x,node y){return x.h<y.h;}
int main()
{
	scanf("%d%d",&r,&c);
	for(int i=1;i<=r;++i)
		for(int j=1;j<=c;++j){
			d[i][j]=1;
			scanf("%d",&m[i][j]);
			a[++tot].h=m[i][j];
			a[tot].x=i,a[tot].y=j;}
	sort(a+1,a+1+tot,cmp);
	while(tot)
	{
		int u,v,i;
		for(i=0;i<4;++i)
		{
			u=a[tot].x+dx[i],v=a[tot].y+dy[i];
			if (u<=0||u>r||v<=0||v>c||m[u][v]>=m[a[tot].x][a[tot].y])
				continue;
			d[u][v]=max(d[u][v],d[a[tot].x][a[tot].y]+1);
		}
		--tot;
	}
	int ans=1;
	for(int i=1;i<=r;++i)
		for(int j=1;j<=c;++j)
			ans=max(ans,d[i][j]);
	printf("%d\n",ans);
	return 0;
}
```



代码运行截图 ==（至少包含有"Accepted"）==

![](https://cdn.luogu.com.cn/upload/image_hosting/hiun7eeu.png)



### 25572: 螃蟹采蘑菇

bfs, dfs, http://cs101.openjudge.cn/practice/25572/

思路：

见题解

代码：

```python
from collections import deque

dire = [(0, 1), (1, 0), (0, -1), (-1, 0)]

def bfs(a, x1, y1, x2, y2):
    visit = set()
    queue = deque([(x1, y1, x2, y2)])
    visit.add((x1, y1, x2, y2))

    while queue:
        xa, ya, xb, yb = queue.popleft()
        for xi, yi in dire:
            nx1, ny1 = xa + xi, ya + yi
            nx2, ny2 = xb + xi, yb + yi
            if 0 <= nx1 < a and 0 <= ny1 < a and 0 <= nx2 < a and 0 <= ny2 < a:
                if (nx1, ny1, nx2, ny2) not in visit and m[nx1][ny1] != 1 and m[nx2][ny2] != 1:
                    queue.append((nx1, ny1, nx2, ny2))
                    visit.add((nx1, ny1, nx2, ny2))
                    if m[nx1][ny1] == 9 or m[nx2][ny2] == 9:
                        return True
    return False

a = int(input())
m = [list(map(int, input().split())) for _ in range(a)]
x1, y1, x2, y2 = -1, -1, -1, -1
found_first = False

for i in range(a):
    for j in range(a):
        if m[i][j] == 5:
            if not found_first:
                x1, y1 = i, j
                m[i][j] = 0
                found_first = True
            else:
                x2, y2 = i, j
                m[i][j] = 0
                break
    if x2 != -1:
        break

check = bfs(a, x1, y1, x2, y2)
print('yes' if check else 'no')
```

### 27373: 最大整数

dp, http://cs101.openjudge.cn/practice/27373/

思路：

见题解，需要字符串作为状态——非常规；如何实现插入新数的位置，有思维难度，依次枚举难度也大

代码：

```python
def f(string):
    if string=='':
        return 0
    else:
        return int(string)
m=int(input())#最大位数
n=int(input())#正整数数量
l=input().split()
#冒泡排序
for i in range(n):
    for j in range(n-1-i):
        if l[j] + l[j+1] > l[j+1] + l[j]:
            l[j],l[j+1] = l[j+1],l[j]
weight=[]#每个元素的位数
for num in l:
    weight.append(len(num))
#dp[i][j]在前i数中选择，不超过j位，最大可能数值
dp=[['']*(m+1) for _ in range(n+1)]
for k in range(m+1):
    dp[0][k]=''#无法组成整数
for q in range(n+1):
    dp[q][0]=''#无法组成整数
for i in range(1,n+1):
    for j in range(1,m+1):
        if weight[i-1]>j:#不能选第i个，因为会超位数
            dp[i][j]=dp[i-1][j]
        else:#可以选第i个也可以不选
                dp[i][j]=str(max(f(dp[i-1][j]),int(l[i-1]+dp[i-1][j-weight[i-1]])))
print(dp[n][m])
```

### 02811: 熄灯问题

brute force, http://cs101.openjudge.cn/practice/02811

思路：

见题解，只需要枚举第一行状态，后续递推

代码：

```python
dx,dy=[0,0,-1,1,0],[0,-1,0,0,1]
def press(light,x,y):
    for i in range(5):
        nx,ny=x+dx[i],y+dy[i]
        if 0<=nx<5 and 0<=ny<6:
            light[nx][ny]^=1 #异或，0^1=1,1^1=0
def doit():
    for first_row in range(64):
        s=[row[:] for row in light]  #不能用.copy()->仅用于一维列表
        solution=[[0]*6 for _ in range(5)]
        for j in range(6):   #第一行有64种开关方式，逐一遍历
            if (first_row >> j)&1:
                solution[0][j]=1
                press(s,0,j)

        for i in range(1,5):
            for j in range(6):
                if s[i-1][j]==1: #根据上一行开着的灯按下一行的按钮
                    solution[i][j]=1
                    press(s,i,j)

        if all(s[4][j]==0 for j in range(6)): #检查最后一行是不是已经熄灭
            for i in solution:
                print(*i)

light=[[int(i) for i in input().split()] for _ in range(5)]
doit()
```



代码运行截图**(T3-T5)** <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/m1862oj5.png)



### 08210: 河中跳房子

binary search, greedy, http://cs101.openjudge.cn/practice/08210/

思路：

初一时候做过，经典二分题。判断某个最短跳跃距离是否合法的标准是达成该目标移除岩石数不大于M。

代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
int L,m,n,a[50005];
bool check(int x)
{
	int cnt=0,j=0;
	for(int i=1;i<=n+1;++i)
		if (a[i]-a[j]<x) ++cnt;
		else j=i;
	if (cnt<=m) return true;
	else return false;
}
int main()
{
	scanf("%d%d%d",&L,&n,&m);
	for(int i=1;i<=n;++i) scanf("%d",&a[i]);
	a[n+1]=L;
	int l=1,r=L,mid;
	while(l<r)
	{
		mid=(l+r+1)>>1;
		if (check(mid)) l=mid;
		else r=mid-1;
	}
	printf("%d",l);
	return 0;
}
```



代码运行截图 <mark>（至少包含有"Accepted"）</mark>

![](https://cdn.luogu.com.cn/upload/image_hosting/kzfvpchp.png)



## 2. 学习总结和收获

<mark>如果作业题目简单，有否额外练习题目，比如：OJ“计概2024fall每日选做”、CF、LeetCode、洛谷等网站题目。</mark>

目前需要回家补状态




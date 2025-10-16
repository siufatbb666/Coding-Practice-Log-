# 树形DP
## 没有上司的舞会（最大独立集问题）
### 题目描述

某大学有 $n$ 个职员，编号为 $1\ldots n$。

他们之间有从属关系，也就是说他们的关系就像一棵以校长为根的树，父结点就是子结点的直接上司。

现在有个周年庆宴会，宴会每邀请来一个职员都会增加一定的快乐指数 $r_i$，但是呢，如果某个职员的直接上司来参加舞会了，那么这个职员就无论如何也不肯来参加舞会了。

所以，请你编程计算，邀请哪些职员可以使快乐指数最大，求最大的快乐指数。

#### 输入格式

输入的第一行是一个整数 $n$。

第 $2$ 到第 $(n + 1)$ 行，每行一个整数，第 $(i+1)$ 行的整数表示 $i$ 号职员的快乐指数 $r_i$。

第 $(n + 2)$ 到第 $2n$ 行，每行输入一对整数 $l, k$，代表 $k$ 是 $l$ 的直接上司。

#### 输出格式

输出一行一个整数代表最大的快乐指数。

#### 输入输出样例 #1

##### 输入 #1

```
7
1
1
1
1
1
1
1
1 3
2 3
6 4
7 4
4 5
3 5
```

##### 输出 #1

```
5
```

#### 说明/提示

##### 数据规模与约定

对于 $100\%$ 的数据，保证 $1\leq n \leq 6 \times 10^3$，$-128 \leq r_i\leq 127$，$1 \leq l, k \leq n$，且给出的关系一定是一棵树。
### 思路
选择树上不相邻的节点使其权值和最大。
设计状态：
&emsp;&emsp;设$h_i$表示员工$i$的快乐指数，$f(i,0/1)$表示员工$i$不参加/参加舞会获得的最大快乐指数。则状态转移开始时，$f(i,0)=0,f(i,1)=h_i$ 。
状态转移方程：
&emsp;&emsp;采用自底向上递推的方式，对于员工$i$，$son_i$表示其下属集合。若其参加舞会，下属均不可参加；若不参加，则下属无限制，此时取最优即可；因此状态转移方程如下：
$$
f(i,j) = 
\begin{cases}
\sum_{k\in son_i}max(f(k,0),f(k,1)) & \text{if } j=0 \\
\sum_{k\in son_i}f(k,0)+h_i & \text{if } j=1
\end{cases}
$$
### ACcode
```cpp
#include<bits/stdc++.h>
using namespace std;

const int N=1e4;
int h[N],n,root;
bool boss[N];
vector<int>son[N];
int f[N][2];

void dfs(int x){
    f[x][0]=0;
    f[x][1]=h[x];

    for(int u : son[x]){
        dfs(u);
        f[x][0]+=max(f[u][0],f[u][1]);
        f[x][1]+=f[u][0];
    }
}
int main(){
    cin>>n;
    for(int i=1;i<=n;i++) cin>>h[i];
    for(int i=1;i<n;i++){
        int l,k;
        cin>>l>>k;
        son[k].push_back(l);
        boss[l]=true;
    }
    for(int i=1;i<=n;i++){
        if(!boss[i]){
            root=i;
            break;
        } 
    } 

    dfs(root);
    cout<<max(f[root][0],f[root][1]);

}
```

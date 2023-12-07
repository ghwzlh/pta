# 运动会

题目大意：T公司的员工层级关系可以表示成一棵树，员工X是员工Y的直接领导，则在树中X是Y的父结点。公司拟组织一场运动会，但为了避免尴尬，每个员工都不想与自己的直接领导一起参赛。假定每个员工都对应一个权重（领导的权重不一定比下属大），请你编写程序，邀请若干员工参赛，使得参赛人员的总权重和最大。

分析：开一个2 * n的二维数组，其中dp(i, 1)表示编号为i的人参加运动会的最大权重，dp(i, 0)表示编号为i的人不参加运动会的最大权重。我们可以得到以下状态转移方程：
$$
dp[i][0] = ∑max(dp[j][0], dp[j][1])
$$

$$
dp[i][1] = ∑dp[j][0] + a[i]
$$

我使用了并查集表示员工关系的树。当然也可以使用其他数据结构来表示。

代码：

```c++
#include<bits/stdc++.h>
using namespace std;
const int M = 3005;
long long n, dp[M][2], dist[M];
bool vis[M], is_have[M];
vector<long long>v;

void dfs(int x)
{
    vis[x] = true;
    for(int i = 1; i <= n; i++)
    {
        if(vis[i] || dist[i] == i || dist[i] != x)
            continue;
        dfs(i);
        dp[x][1] += dp[i][0];
        dp[x][0] += max(dp[i][0], dp[i][1]);
    }
    return ;
}

int main()
{
    memset(vis, false, sizeof vis);
    memset(is_have, false, sizeof is_have);
    memset(dp, 0, sizeof dp);
    cin >> n;
    for(int i = 1; i <= n; i++)
        cin >> dp[i][1];
    for(int i = 1; i <= n; i++)
        dist[i] = i;
    for(int i = 1; i <= n; i++)
    {
        long long x, y;
        cin >> x >> y;
        is_have[x] = true;
        dist[x] = y;
    }
    for(int i = 1; i <= n; i++)
    {
        if(!is_have[i])
        {
            dfs(i);
            v.push_back(dp[i][0]);
            v.push_back(dp[i][1]);
        }
    }
    sort(v.begin(), v.end());
    cout << v[v.size() - 1];
    return 0;
}
```


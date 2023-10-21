# 区间DP

​		用区间的两个端点来描述状态和处理状态的转移。区间DP的计算量比较大。复杂度一般在O(n^3)左右。下面给出一些题目，并给出模板代码。

# 石子合并 · 一

题目：由n堆石子排成一排，其编号为1，2，3……，n。每堆石子有一定的质量mi(mi<=1000),现在要将这n堆石子合并成一堆。每次只能合并相邻的两堆，合并的代价为这两堆石子的质量之和，合并后与这两堆石子相邻的石子将和新堆相邻，由于合并顺序的不同，导致合并成一堆石子的总代价也不同，请求出最少的代价将所有石子合并为一堆。

分析：在计算大区间（i, j）的最优值时，我们可以把他看作由（i, k）和（k + 1, j）两个最优值转化而来，这样再细分直至区间（i, i + 1）为止。我们用数组dp来表示这个集合，由以下状态转移方程：
$$
dp[i, j] = min(dp[i][j], dp[i][k] + dp[k +1][j] + sum[j] - sum[i - 1])      
$$
其中k从i一直枚举到j - 1。

代码：

```c++
#include<bits/stdc++.h>
using namespace std;
const int M = 1e3 + 5;
int dp[M][M], v[M], t, sum[M];
int main()
{
    memset(sum, 0, sizeof sum);
    memset(dp, 0x3f, sizeof dp);
    cin >> t;
    for(int i = 1; i <= t; i++)
    {
        cin >> v[i];
        sum[i] = sum[i - 1] + v[i];
    }
    for(int i = 1; i <= t; i++)
        dp[i][i] = 0;
//这种区间dp的书写方式是比较推荐的
    for(int len = 2; len <= t; len++)//枚举区间的长度
    {
        for(int i = 1; i + len - 1 <= t; i++)//枚举起始端点
        {
            int j = i + len - 1;//计算结束端点
            for(int k = i; k < j; k++)//枚举每一个k
            {
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k + 1][j] + sum[j] - sum[i - 1]);
            }
        }
    }
    cout << dp[1][t] << endl;
    return 0;
}
```

# 石子合并 · 二

​		题目：在一个圆形操场的四周摆放 N 堆石子，现要将石子有次序地合并成一堆，规定每次只能选相邻的2堆合并成新的一堆，并将新的一堆的石子数，记为该次合并的得分。

试设计出一个算法,计算出将 N 堆石子合并成 1 堆的最小得分和最大得分。

​		分析:这道题目比上面那道略微升级，这里的情形是环的情况，上面其实是破环成链的特殊情况。

面对这种情况我们有两种方法：1.枚举每一个端点（从这里将他破环成链）,再像链的情况一样写代码即可。这样的写法时间复杂度会到达O(n^4)。2.扩充这个环，使长度达到2 * n - 1。比如原来是（4, 5, 9, 4）,扩充完后成为（4, 5, 9, 4, 4, 5, 9）。这样也可以看做是链的情况。只不过最后答案是dp(i, i + n - 1)中的最优值。这里我使用的是方法2。

从中我们可以看到，如果成环的情况不好做，我们可以破环成链，方便我们的分析。

代码：

```c++
#include<bits/stdc++.h>
using namespace std;
const int M = 3600;
int v[M], sum[M], n, dp[M][M];

void get_min(int t)
{
    for(int len = 2; len <= t; len++)
    {
        for(int i = 1; i + len - 1 <= 2 * t; i++)
        {
            int j = i + len - 1;
            for(int k = i; k < j; k++)
            {
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k + 1][j] + sum[j] - sum[i - 1]);
            }
        }
    }
    vector<int> v;
    for(int i = 1; i <= t; i++)
        v.push_back(dp[i][i + t - 1]);
    sort(v.begin(), v.end());
    cout << v[0] << endl;
}

void get_max(int t)
{
    for(int len = 2; len <= t; len++)
    {
        for(int i = 1; i + len -1 <= 2 * t; i++)
        {
            int j = i + len - 1;
            for(int k = i; k < j; k++)
            {
                dp[i][j] = max(dp[i][j], dp[i][k] + dp[k + 1][j] + sum[j] - sum[i - 1]);
            }
        }
    }
    vector<int> v;
    for(int i = 1; i <= t; i++)
        v.push_back(dp[i][i + t - 1]);
    sort(v.begin(), v.end());
    cout << v[v.size() - 1] << endl;
}
int main()
{
    memset(dp, 0x3f, sizeof dp);
    memset(sum, 0, sizeof sum);
    cin >> n;
    for(int i = 1; i <= n; i++)
    {
        cin >> v[i];
        sum[i] = sum[i - 1] + v[i];
    }
    for(int i = n + 1; i <= 2 * n; i++)
    {
        v[i] = v[i - n];
        sum[i] = sum[i - 1] + v[i];
    }
    for(int i = 0; i <= 2 * n; i++)
        dp[i][i] = 0;
    get_min(n);
    memset(dp, 0, sizeof dp);
    get_max(n);
    return 0;
}
```



最后我们给出区间DP的模板代码：

```c++
for (len = 2; len <= n; len++)//枚举区间长度
  for (i = 1; i + len - 1 <= n; i++)//枚举区间起始端点 
  {
    int j = len + i - 1;//计算区间结束端点
    for (k = i; k < j; k++)//枚举区间内部每个长度
      f[i][j] = max(f[i][j], f[i][k] + f[k + 1][j] + sum[j] - sum[i - 1]);//这里可以看情况区最大和最小
  }
```


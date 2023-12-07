# 基础dp

# 数塔（洛谷P1216）

题目大意：数塔如图所示，若每一步只能走到相邻的结点（图中有数字的方格），则从最顶层走到最底层所经过的所有结点的数字之和最大是多少？

分析：基础的dp，水题。

代码：

```c++
#include<bits/stdc++.h>
using namespace std;

const int M = 1010;
int dp[M][M], r, v[M][M];

void slove()
{
    cin >> r;

    for (int i = 0; i < r; i++)
        for (int j = 0; j <= i; j++)
            cin >> v[i][j];

    dp[0][0] = v[0][0];
    for (int i = 1; i < r; i++)
        dp[i][0] = v[i][0] + dp[i - 1][0];

    for (int i = 1; i < r; i++)
        for (int j = 1; j <= i; j++)
            dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - 1]) + v[i][j];
    
    sort(dp[r - 1], dp[r - 1] + r);

    cout << dp[r - 1][r - 1] << endl;;
    return ;
}

int main()
{
    int t;
    cin >> t;
    while(t--)
        slove();
    return 0;
}
```


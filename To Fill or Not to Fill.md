                                                  To Fill or Not to Fill

​		对于这道题，很明显是使用贪心算法。

​		我们可以发现要获得最小的费用，只要保证每次购买油时的费用尽量少，因此，这道题可以分为三种情况：

第一：我们在能从当前加油站所行驶到的最远距离范围内进行搜索。若能找到费用最小且比当前加油站费用低（这一点很重要）的站点，那么就只需要在当前站点购买足够到达该站点的油即可。

第二：我们在能从当前加油站所行驶到的最远距离范围内进行搜索。若找不到比当前加油站费用低的站点，就选择在范围内的费用最低的站点，但我们要在当前站点将油箱加满（这是因为在能从当前加油站所行驶到的最远距离范围内找不到比当前加油站费用低的站点，而我们要保证费用越低越好）。

第三：我们在能从当前加油站所行驶到的最远距离范围内进行搜索。若找不到能到达的站点，则加满油，行驶出最大距离后就直接输出。

下面是代码：

```c++
#include<bits/stdc++.h>
using namespace std;

const int M = 1000;
const int INF = 0x3f3f3f3f;

double a, b, c, d;
struct node {
    double cost;
    double dis;
};
struct node v[M];
bool cmp(struct node a, struct node b)
{
    return a.dis < b.dis;
}
int main()
{
    cin >> a >> b >> c >> d;   //a为油箱总容量   b为路程总长度   c为每单位油能走的路程  d为加油站总数
    for (int i = 0; i < d; i++)
        cin >> v[i].cost >> v[i].dis;
    
    sort(v, v + (int)d, cmp);
    v[(int)d].cost = 0;
    v[(int)d].dis = b;

    if (v[0].dis > 0)
    {
        printf("The maximum travel distance = 0.00\n");
        return 0;
    }
    double max_dis = a * c;  //max_dis为加满油能走的最长距离
    double min_cost = v[0].cost;
    double ans = 0, sum = 0, c1 = 0;
    int j = 0, tt = 0;
    while (ans < b)
    {
        int q = 0, k;
        for (k = j + 1; v[k].dis - ans <= max_dis; k++)
        {
            if (v[k].cost <= min_cost)
            {
                q = k;
                break;
            }
        }
        if (q)
        {
            ans = v[q].dis;
            sum += ((v[q].dis - v[j].dis) / c - c1) * min_cost;
            c1 = 0;
            j = q;
            min_cost = v[q].cost;
        }
        else
        {
            int g = j + 1;
            for (k = j + 1; v[k].dis - ans <= max_dis; k++)
            {
                if (v[k].cost < v[g].cost)
                    g = k;
            }
            if (v[g].dis - v[j].dis <= max_dis)
            {
                ans = v[g].dis;
                sum += (a - c1) * min_cost;
                c1 = a - (v[g].dis - v[j].dis) / c;
                j = g;
                min_cost = v[g].cost;
            }
            else
            {
                tt = 1;
                ans += max_dis;
                break;
            }

        }
    }
    if (tt)
        printf("The maximum travel distance = %.2lf", ans);
    else
        printf("%.2lf", sum);
    return 0;
}
```


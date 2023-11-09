```c++
#include<iostream>
#include<vector>
#include<algorithm>
using namespace std;

int n, k;
int main()
{
    cin >> n >> k;
    vector<int> v;
    for(int i = 0; i < n; i++)
    {
        int x;
        cin >>x;
        v.emplace_back(x);
    }
    sort(v.begin(), v.end());
    cout << v[k - 1];
    return 0;
}
```


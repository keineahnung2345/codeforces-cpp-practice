# official solution - math
GNU C++17 140 ms	400 KB

GNU C++11	187 ms	400 KB

Note: GNU C++11 gives WA for `ans += abs(a[i] - pow(x, i));`, it needs to be `ans += abs(a[i] - llround(pow(x, i)));`!!

```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>
#include <cmath>
 
using namespace std;
 
long long f(vector<int>& a, long long x){
    long long ans = 0;
    
    for(int i = 0; i < a.size(); ++i){
        ans += abs(a[i] - pow(x, i));
    }
    
    return ans;
}
 
int main()
{
    int n;
    cin >> n;
    
    vector<int> a(n);
    
    for(int i = 0; i < n; ++i){
        cin >> a[i];
    }
    
    sort(a.begin(), a.end());
    
    long long ubound = f(a, 1) + a.back();
    long long minx;
    long long min_cost = LLONG_MAX;
 
    for(long long x = 1; pow(x, n-1) <= ubound && x <= pow(LLONG_MAX, 1.0/(n-1)); ++x){
        long long cost = f(a, x);
        // cout << x << " : " << cost << endl;
        if(cost < min_cost){
            min_cost = cost;
            minx = x;
            // cout << minx << " : " << min_cost << endl;
        }
    }
    
    cout << min_cost << endl;
    
    return 0;
}
```

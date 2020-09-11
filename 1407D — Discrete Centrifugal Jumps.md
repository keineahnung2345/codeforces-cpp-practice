# official solution - monotonic stack + dp

There are three jump method: 
- i+1=j
- max(hi+1,…,hj−1)<min(hi,hj)
- max(hi,hj)<min(hi+1,…,hj−1).

Jump method 2, from `x` to `y`, and all buildings btw them are strictly lower. 

Case 1: `h[x] <= h[y]`, `y` must be the first building  s.t. `h[x] <= h[y]`. 
Otherwise, there will be another building not stricter lower than `min(h[x],h[y])` btw `x` and `y`.

Case 2: `h[x] >= h[y]`, `x` must be the first building to `y`'s left s.t. `h[x] >= h[y]`. 
Otherwise, there will be another building >= `h[y]` btw `x` and `y`.

Jump method 3, from `x` to `y`, and all buildings btw them are strictly higher. 

Case 1: `h[x] >= h[y]`, `y` must be the first building  s.t. `h[x] >= h[y]`.
Otherwise, there will be another not stricter higher than `min(h[x], h[y]` btw `x` and `y`.

Case 2: `h[x] <= h[y]`, `x` must be the first building to `y`'s left s.t. `h[x] < h[y]`. 
Otherwise, there will be another building <= `h[y]` btw `x` and `y`.

```cpp
#include <iostream>
#include <vector>
#include <stack>
#include <climits>
 
using namespace std;
 
int main()
{
    int n;
    
    cin >> n;
    
    vector<int> h(n);
    
    for(int i = 0; i < n; ++i){
        cin >> h[i];
    }
    
    stack<int> stk;
    vector<int> lge(n), lle(n), rge(n), rle(n);
    
    for(int i = 0; i < n; ++i){
        //current index in stack must be to the left of i
        /*
        the index pointing to the building < h[i] cannot be the valid x,
        also since h[i] > h[stk.top()], so later when we are at j(>i),
        we may stop at h[i] and don't go further to find h[stk.top()]
        */
        while(!stk.empty() && h[stk.top()] < h[i]){
            stk.pop();
        }
        
        if(stk.empty()) lge[i] = -1;
        else lge[i] = stk.top();
    	stk.push(i);
    }
    stk = stack<int>();
    
    for(int i = 0; i < n; ++i){
        while(!stk.empty() && h[stk.top()] > h[i]){
            stk.pop();
        }
        
        if(stk.empty()) lle[i] = -1;
        else lle[i] = stk.top();
        stk.push(i);
    }
    stk = stack<int>();
    
    for(int i = n-1; i >= 0; --i){
        while(!stk.empty() && h[stk.top()] < h[i]){
            stk.pop();
        }
        
        if(stk.empty()) rge[i] = -1;
        else rge[i] = stk.top();
        stk.push(i);
    }
    stk = stack<int>();
    
    for(int i = n-1; i >= 0; --i){
        while(!stk.empty() && h[stk.top()] > h[i]){
            stk.pop();
        }
        
        if(stk.empty()) rle[i] = -1;
        else rle[i] = stk.top();
        stk.push(i);
    }
    stk = stack<int>();
    
    vector<vector<int>> jumps(n);
    for(int i = 0; i < n; ++i){
        if(lge[i] != -1) jumps[lge[i]].push_back(i);
        if(lle[i] != -1) jumps[lle[i]].push_back(i);
        if(rge[i] != -1) jumps[i].push_back(rge[i]);
        if(rle[i] != -1) jumps[i].push_back(rle[i]);
    }
    
    vector<int> dp(n, INT_MAX);
    dp[0] = 0;
    for(int from = 0; from < n; ++from){
        for(int to : jumps[from]){
            dp[to] = min(dp[to], dp[from]+1);
        }
    }
    
    cout << dp[n-1] << endl;
 
    return 0;
}
```

For jump method 1, we don't need to explicitly consider it, because in the following code:

```cpp
if(rge[i] != -1) jumps[i].push_back(rge[i]);
if(rle[i] != -1) jumps[i].push_back(rle[i]);
```

`h[i+1]` is either >= or <= `h[i]`, so `i` can always jump to `i+1`.

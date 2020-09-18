# official solution - math

Note: cannot do sorting after we take MOD!!

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

int MOD = 1e9+7;

void dfs(int u, int p, int& n, vector<vector<int>>& graph, vector<int>& sz, vector<long long>& w){
    sz[u] = 1;
    
    for(int& v : graph[u]){
        if(v == p) continue;
        dfs(v, u, n, graph, sz, w);
        sz[u] += sz[v];
        //count of simple path passing through (u, v)
        //cannot take MOD here, because later there is a sorting!!
        w.push_back(1LL * sz[v] * (n-sz[v]));
    }
}

int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n;
        cin >> n;
        
        vector<vector<int>> graph(n);
        
        for(int i = 0; i < n-1; ++i){
            int u, v;
            cin >> u >> v;
            //1-based to 0-based
            --u; --v;
            graph[u].push_back(v);
            graph[v].push_back(u);
        }
        
        vector<int> sz(n);
        
        //(edge id, count of simple path passing it)
        //there will be n-1 edges in a tree of n nodes
        vector<long long> w; 
        dfs(0, -1, n, graph, sz, w);
        
        int m;
        cin >> m;
        
        vector<long long> p(m);
        for(int i = 0; i < m; ++i){
            cin >> p[i];
        }
        
        sort(p.begin(), p.end());
        
        int edgeCnt = n-1;
        
        if(edgeCnt > m){
            //not enough prime numbers
            for(int i = m; i < edgeCnt; ++i){
                p.push_back(1);
            }
            sort(p.begin(), p.end());
        }else{
            //too many prime numbers
            //merge p[edgeCnt-1:m-1] into p[edgeCnt-1]
            for(int i = edgeCnt; i < m; ++i){
                //here we take MOD, so later we cannot perform sorting!!
                p[edgeCnt-1] = (p[edgeCnt-1] * p[i]) % MOD;
            }
            //after this, we only care p[0:edgeCnt-1]
            //don't need to sort!!
        }
        
        sort(w.begin(), w.end());
        
        long long ans = 0;
        
        //sum w*p for n-1(, not n!!) edges
        for(int i = 0; i < edgeCnt; ++i){
            // cout << w[i] << ", " << p[i] << endl;
            ans = (ans + ((w[i] % MOD) * p[i]) % MOD) % MOD;
        }
        
        cout << ans << endl;
    }

    return 0;
}
```

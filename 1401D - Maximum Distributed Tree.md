# official solution - math, dfs, dp, greedy

Let `f(u,v)` be the sum of numbers on the simple path `(u,v)`, the problem requires us to find out the sum of `f(u,v)` for all pairs of nodes. 
Changing a point of view, the answer is equivalent to the following: in one time we consider an edge, calculate how many simple path passing through it, 
multiply this number by the edge's number, and then sum for all edges.

Let `w[i]` be the number of simple path passing through the edge `i`, `z[i]` be the number of that edge, `i` in `[0, n-1]`. 
`w[i]` is the product of the number of nodes after cutting this edge, and it can be calculated by using dfs.
`z[i]` are chosen from the `m` input primes, now there are two cases:
- m <= n-1(edge count):
  In this case, we need to add some 1s, so that the size of `z` will be `n-1`.
- m > n-1:
  In this case, we combine some of the primes(by product) so there remains exactly `n-1` primes. 
  What primes are we are going to combine? Combine the `m-n+2` largest primes together.(This can be proven: https://codeforces.com/blog/entry/81700)
  
Now there is only one problem: how to match the `n-1` `w[i]` and `z[i]`? We simply first sort both of them and match them one by one.(This can be proven: https://codeforces.com/blog/entry/81700)

Note1: `w` is the container of int * int, so it has to be long long; `p[edgeCnt-1]` is the product of `p[edgeCnt-1:m-1]`, so it has to be long long, too.

Note2: cannot do sorting after we take MOD!!

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

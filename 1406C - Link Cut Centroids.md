# official solution - tree centroid

Some references:

[Centroid Decomposition of Tree](https://www.geeksforgeeks.org/centroid-decomposition-of-tree/)

[A Visual Introduction to Centroid Decomposition](https://medium.com/carpanese/an-illustrated-introduction-to-centroid-decomposition-8c1989d53308)

[The way to find the centroids of a tree](https://codeforces.com/blog/entry/57593)

```cpp
#include <vector>
#include <iostream>
#include <functional> //function keyword

using namespace std;

void findCentroids(vector<vector<int>>& graph, vector<int>& sz, vector<int>& parents, vector<int>& centroids){
    int n = graph.size();
    
    function<void (int, int)> dfs = [&](int u, int p){
        // cout << "visit " << u << endl;
        sz[u] = 1;
        parents[u] = p;
        bool is_centroid = true;
        
        for(int& v : graph[u]){
            if(v == p) continue;
            dfs(v, u);
            //accumulate its subtree v's size
            sz[u] += sz[v];
            /*
            after removing u, 
            if any component(here it's a subtree rooted at v)'s size > n/2,
            then u is not centroid
            */
            if(sz[v] > (n >> 1)) is_centroid = false;
        }
        /*
        in the tree, the part above u is also considered a component,
        if this component's size > n/2, then u is not centroid
        */
        if(n - sz[u] > (n>>1)) is_centroid = false;
        
        if(is_centroid) centroids.push_back(u);
    };
    
    dfs(0, -1);
};

void findLeaf(int u, int p, vector<vector<int>>& graph, vector<int>& sz, int& leaf){
    if(sz[u] == 1){
        //u is leaf node
        leaf = u;
        return;
    }
    
    for(int& v : graph[u]){
        if(v == p) continue;
        findLeaf(v, u, graph, sz, leaf);
        if(leaf != -1) return;
    }
}

int main(){
    int t;
    cin >> t;
    
    while(t-- > 0){
        int n;
        cin >> n;
        
        vector<vector<int>> graph(n);
        
        for(int i = 0; i < n-1; ++i){
            int x, y;
            cin >> x >> y;
            graph[x-1].push_back(y-1);
            graph[y-1].push_back(x-1);
        }
        
        vector<int> sz(n), parents(n), centroids;
        findCentroids(graph, sz, parents, centroids);
        
        // cout << "centroids: " << centroids.size() << endl;
        
        if(centroids.size() == 1){
            cout << 1 << " " << graph[1-1][0]+1 << endl;
            cout << 1 << " " << graph[1-1][0]+1 << endl;
        }else{
            //centroids.size() == 2
            int x = centroids[0], y = centroids[1];
            if(parents[y] != x) swap(x, y);
            //x is y's parent
            
            int leaf = -1;
            findLeaf(y, x, graph, sz, leaf);
            
            //cut the leaf edge
            cout << leaf+1 << " " << parents[leaf]+1 << endl;
            //append it to the tree x
            cout << x+1 << " " << leaf+1 << endl;
        }
    }
    return 0;
}
```

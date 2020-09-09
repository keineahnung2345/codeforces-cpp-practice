# official solution
358 ms	3400 KB

In this problem, we need to :
- find the distance of two points in a (not binary) tree
- find the "diameter" of the tree
  - https://www.geeksforgeeks.org/diameter-tree-using-dfs/

```cpp
#include <iostream>
#include <vector>
#include <climits>

using namespace std;

int getDist(vector<vector<int>>& adjList, int n, int cur, int parent, int b){
    if(cur == b) return 0;

    int res = n;

    for(int& nei : adjList[cur]){
        if(nei != parent){
            res = min(res, 1 + getDist(adjList, n, nei, cur, b));
        }
    }
    
    return res;
};

int getDist(vector<vector<int>>& adjList, int n, int a, int b){
    return getDist(adjList, n, a, -1, b);
};

void dfs(vector<vector<int>>& adjList, int n, int cur, int parent, int curdist, int& x, int& xdist){
    // ++curdist;
    // cout << cur << "'s dist: " << curdist << endl;
    for(int& nei : adjList[cur]){
        // cout << "nei: " << nei << endl;
        if(nei != parent){
            //parent is visited
            int neidist = curdist+1;
            // cout << cur << " -> " << nei << ", dist: " << neidist << endl;
            if(neidist >= xdist){
                x = nei;
                xdist = neidist;
            }
            dfs(adjList, n, nei, cur, neidist, x, xdist);
        }
    }
};

int getDiameter(vector<vector<int>>& adjList, int n){
    /*
    1. start from a random node and find the farthest node X
    2. start from X and find its farthest node Y
    the distance btw X and Y is the tree's diameter?
    */
    int x, xdist = INT_MIN;
    //node index is 1-based, 0 is meaningless
    dfs(adjList, n, 1, 0, 0, x, xdist);
    // cout << "after first dfs: 1 -> " << x << ": " << xdist << endl;
    int oldx = x;
    dfs(adjList, n, x, 0, 0, x, xdist);
    // cout << "after second dfs: " << oldx << "-> " << x << ": " << xdist << endl;
    return xdist;
};

int main(){
    int t;
    int n, a, b, da, db;
    vector<vector<int>> adjList;

    cin >> t;

    while(t-- > 0){
        cin >> n >> a >> b >> da >> db;
        
        adjList = vector<vector<int>>(n+1);
        
        //n-1 edges!!
        for(int i = 0; i < n-1; ++i){
            int u, v;
            cin >> u >> v;
            adjList[u].push_back(v);
            adjList[v].push_back(u);
        }
        
        // for(int i = 1; i <= n; ++i){
        //     cout << i << "->";
        //     for(int& nei : adjList[i]){
        //         cout << nei << " ";
        //     }
        //     cout << endl;
        // }

        //use dfs to find the distance btw a and b
        int dist = getDist(adjList, n, a, b);
        int diameter = getDiameter(adjList, n);
        
        // cout << a << "->" << b << " : " << dist << endl;
        // cout << "diameter: " << diameter << endl;
        
        if(dist <= da){
            cout << "Alice" << endl;
        }else if(da*2 >= diameter){
            cout << "Alice" << endl;
        }else if(db > 2*da){
            cout << "Bob" << endl;
        }else{
            //db <= 2*da
            cout << "Alice" << endl;
        }
		// cout << "============" << endl;
    }
    return 0;
}
```

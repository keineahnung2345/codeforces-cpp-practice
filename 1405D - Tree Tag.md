# official solution
358 ms	3400 KB

Depending on dist(a,b), `da`, `db` and the tree's diameter, there are four cases:
- dist(a,b) <= da: Alice can catch Bob at her first move
- da * 2 >= diameter: diameter here means the count of edges in longest simple path of the tree. In Alice's first move, she moves to the center of the tree, later she can reach any vertex in the tree in just one move.
- db > 2 * da: Bob will win in this case. Since we are not in case 1, Bob won't lose in before his first move. And since we are not in case 2, there exist a vertex v that Alice cannot reach in one move, i.e. dist(a, v) = da+1. Depending on whether Bob is at such vertex v, there are two cases:
  - Bob is at v: then he can simply stay there
  - Bob is not at v: then he can move to v in one step, this can be proven by dist(b,v) <= dist(b,a) + dist(a,v)  <= da + (da+1) <= db
    - The first inequality is by triangle inequality
    - The second inequality: dist(b,a) <= da? and dist(a,v) = da+1. dist(b,a) <= da is confusing, have posted a question in the [comment](https://codeforces.com/blog/entry/82366?#comment-693311).
- db <= 2 * da: (copied from editorial) In this case, Alice's strategy will be to capture Bob whenever possible or move one vertex closer to Bob otherwise. Let's prove that Alice will win in a finite number of moves with this strategy. Let's root the tree at a. Bob is located in some subtree of a, say with k vertices. Alice moves one vertex deeper, decreasing Bob's subtree size by at least one vertex. Since db≤2da, Bob cannot move to another subtree without being immediately captured, so Bob must stay in this shrinking subtree until he meets his inevitable defeat.

So in this problem, we need to :
- find the distance of two points in a (not binary) tree
  - This can be done by dfs, in the following code, we use the variable `parent` to avoid revisiting a vertex, its effect is same as a `visited` array
- find the "diameter" of the tree
  - Note that "diameter" in this problem is defined by edge count
  - In https://www.geeksforgeeks.org/diameter-tree-using-dfs/ , "diameter" is defined by vertex count
  - To find diameter, we first start from a random vertex, and find a vertex farthest from it, called that vertex `x`. Then start from `x`, find a vertex farthest from it, their distance is the diameter of the tree.

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

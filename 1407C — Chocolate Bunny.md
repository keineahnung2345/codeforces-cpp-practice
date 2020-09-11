# official solution
187 ms	0 KB

There is a permutation of size `n`, it requires us to guess the permutation using at most `2*n` queries. 

The strategy is as following, while we make queries, we maintain a index `maxidx`, pointing to the maximum in the permutation. 
In each iteration, we query (x,y) and then (y,x), by doing this, we can find the minimum of `p[x]` and `p[y]`, why is that?

Claim `(a mod b) > (b mod a) <-> a < b`, this can be proved:
When `a < b`, `a mod b` will be `a`, and `b mod a` will be in `[0,a-1]`, so  `b mod a < a`. Also `(a mod b) == (b mod a) == 0 <-> a == b` .

By the theorem above, we can induce which is bigger in `a` and `b` by comparing the return value of the two queries. 
Also, say `query(x,y) = u`, `query(y,x) = v`, we also know that if `p[x] = u ` if `u` is larger.

After iterating for `n-1` times(each time we do two queries), we know the `n-1` small elements in the permutation, 
so the remaining element pointed by `maxidx` must be `n`.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>
 
using namespace std;

int ask(int x, int y){
    cout << "? " << x << " " << y << endl;
    int res;
    cin >> res;
    return res;
}

int main()
{
    int n;
    
    cin >> n;
    
    //0 for padding, 1-based
    vector<int> ans(n+1);
    int maxidx = 1;
    
    for(int i = 2; i <= n; ++i){
        int a = ask(maxidx, i); //p_maxidx % p_i
        int b = ask(i, maxidx); //p_i % p_maxidx
        // cout << "a: " << a << ", b: " << b << endl;
        
        //a cannot equal to b because all elements in p are unique
        if(a > b){
            //p_maxidx < p_i so a = p_maxidx % p_i = p_maxidx
            //here maxidx points to the smaller element
            ans[maxidx] = a;
            // cout << "a larger, ans[" << maxidx << "] = " << a <<  endl;
            maxidx = i;
        }else{
            //a < b
            //p_i < p_maxidx so b = p_i % p_maxidx = p_i
            //here i points to the smaller element
            ans[i] = b;
            // cout << "b larger, ans[" << i << "] = " << b <<  endl;
        }
    }
    
    //maxidx points to the largest element
    ans[maxidx] = n;
    // cout << "ans[" << maxidx << "] = " << n <<  endl;
    
    cout << "! ";
    for(int i = 1; i <= n; ++i){
        cout << ans[i] << " ";
    }
    cout << endl;

    return 0;
}
```

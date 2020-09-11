# official solution
187 ms	0 KB

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

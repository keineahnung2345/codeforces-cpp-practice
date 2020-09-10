This problem gives us an input array `a`, and it requires us to reorder it to create the array `b` so that array `c` is lexicographically maximal.
The definition of array `c` is `ci=GCD(b0,…,bi)`.

# maintain cumulative gcd array
The largest element in `a` gives us the largest gcd, so the first element of `c` must be the largest element in `a`. 
We keep an array `gcds` containing the cumulative gcd, then in each iteration, we choose the element in `a`(which is `ai`) that will maximize `ci`.

31 ms	200 KB

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
 
using namespace std;
 
int main()
{
    int t, n;
    vector<int> A(1000);
    vector<int> gcds(1000);
    
    cin >> t;
    
    while(t-- > 0){
        cin >> n;
        
        for(int i = 0; i < n; ++i){
            cin >> A[i];
            gcds[i] = A[i];
        }
        
        int cur_gcd = *max_element(A.begin(), A.begin()+n);
        int tmp = n;
        while(tmp-- > 0){
            for(int i = 0; i < n; ++i){
                if(gcds[i] != -1){
                    gcds[i] = __gcd(gcds[i], cur_gcd);
                }
            }
            
            auto it = max_element(gcds.begin(), gcds.begin()+n);
            
            int idx = (it-gcds.begin());
            
            cur_gcd = __gcd(cur_gcd, gcds[idx]);
            
            cout << A[idx] << " ";
            
            gcds[idx] = -1;
        }
        
        cout << endl;
    }
 
    return 0;
}
```

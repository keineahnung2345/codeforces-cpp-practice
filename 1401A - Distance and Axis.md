62 ms	12 KB

time: O(1)
 
If n < k, move A from n to k, then put B at k, so OB = k and AB = 0, OB-AB = k.

If n >= k, say we put B at some place m. Then AB - OB = (n-m) - m = k. So m = (n-k)/2, this m exist if n-k is 2's multiple, if not, we can move A's position by one.

```cpp
#include <iostream>
 
using namespace std;
 
int main(){
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n, k;
        cin >> n >> k;
        
        if(n >= k){
            cout << (n-k)%2 << endl;
        }else{
            cout << k-n << endl;
        }
    }
    return 0;
}
```

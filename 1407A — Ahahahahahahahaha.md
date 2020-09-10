This problem gives us an array and requires us to remove at most n/2 elements of it to make `a[0]-a[1]+a[2]-a[3]+... = 0`. 
The number of removed elements won't necessarily be minimum.

# count zeros and ones
30 ms	0 KB

This problem is like a brian teaser, when there are more zeros than ones, we remove all ones; otherwise, we remove 0 or 1 one to make the count of remaining ones even.

```cpp
#include <iostream>
#include <vector>
 
using namespace std;
 
int main()
{
    int t, n;
    int A[1000];
    
    cin >> t;
    
    while(t-- > 0){
        cin >> n;
        
        int ones = 0, zeros = 0;
        for(int i = 0; i < n; ++i){
            cin >> A[i];
            if(A[i]) ++ones;
            else ++zeros;
        }
        
        if(zeros >= ones){
            //remove all 1s
            cout << zeros << endl;
            for(int i = 0; i < zeros; ++i){
                cout << 0 << " ";
            }
            cout << endl;
        }else{
            //ones > zeros, so ones > n/2 >= 1
            if(ones&1) --ones;
            cout << ones << endl;
            for(int i = 0; i < ones; ++i){
                cout << 1 << " ";
            }
            cout << endl;
        }
    }
 
    return 0;
}
```

# two pass

249 ms	400 KB

An operation is defined as `--A[i]` and `++A[j]` for `i` not equal to `j`. 
The operation is free if `i` < `j`(move values toward right), otherwise it cost one coin. 
An important thing to notice is that the cost is not dependent on the distance of `i` and `j`, 
so later when we perform the not free operation, we always decrease `A[n-1]` and add to `A[0]`.

Following is the algorithm, in the first pass, we do free operations as more as possible. 
When we go forward, we simply collect and clean the values of positive `A[i]` (as `quota`), and then use the `quota` when we later meet negative `A[i]`.

After the first pass, there could still exist some negative `A[i]`(because the `quota` accumulated before `A[i]` is not enough to clear it). 
So now we need to do the operation in opposite direction, which will cost coins. 
First calculate the coins we need, which is the sum of negative `A[i]` multiplied by -1, and then do operation: decreasing `A[n-1]` by `ans` and add it to `A[0]`. 

The next step is not shown in the code, it just do several free operations from `A[0]` to those negative `A[j]`'s to make the whole array 0. 

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    int t;
    
    cin >> t;
    
    vector<int> A(1e5);
    
    while(t-- > 0){
        int n;
        
        cin >> n;
        
        for(int i = 0; i < n; ++i){
            cin >> A[i];
        }
        
        long long quota = 0;
        for(int i = 0; i < n; ++i){
            if(A[i] > 0){
                quota += A[i];
                A[i] = 0;
            }else if(quota > 0){
                //A[i] <= 0
                long long used = min(quota, (long long)(-A[i]));
                quota -= used;
                A[i] += used;
            }
        }
        
        long long ans = 0;
        for(int i = 0; i < n; ++i){
            if(A[i] < 0) ans += -A[i];
        }
        
        cout << ans << endl;
    }

    return 0;
}
```

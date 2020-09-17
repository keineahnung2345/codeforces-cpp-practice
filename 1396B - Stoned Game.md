# official solution - math
46 ms	0 KB

time: O(N)

 - case 1: There is a pile > floor(S/2)
 
   The first player wins by always choosing this pile.

 - case 2: Every pile <= floor(S/2), and S is even
 
   Label the `S` stones from `0` to `S-1`, the labels of the stones in the same pile are continuous.
We make the stone `i` and `i+S/2` a pair for all `i` in `[0,S/2-1]`.
Then when first player choose stone`i`, the second player just choose its matching stone, 
that stone must be in a different pile as stone `i` because every pile's size is at most `S/2`. So second player will win.

 - case 3: Every pile <= floor(S/2), and S is odd
 
   After the first player pick a stone, it becomes case 2, so first player will win.

```cpp
#include <vector>
#include <iostream>

using namespace std;

int main(){
    int t;
    cin >> t;
    
    while(t-- > 0){
        int n;
        cin >> n;

        vector<int> a(n);
        long long S = 0;
        for(int i = 0; i < n; ++i){
            cin >> a[i];
            S += a[i];
        }

        bool large_pile = false;
        for(int i = 0; i < n; ++i){
            if(a[i] > (S>>1)){
                large_pile = true;
                break;
            }
        }

        if(large_pile || S&1){
            cout << "T" << endl;
        }else{
            cout << "HL" << endl;
        }
    }
    
    return 0;
}
```

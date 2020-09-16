# counter
46 ms	0 KB

First create a counter for the input set. Then start from 0, 1, ..., assign the numbers to the set A, if we find a missing number, we recognize it as `mex(A)`.
Then do the same thing to find `mex(B)`.

```cpp
#include <iostream>
#include <vector>
#include <climits>
 
using namespace std;
 
int main()
{
    int t;
    vector<int> counter(101, 0);
    
    cin >> t;
    
    while(t-- > 0){
        counter = vector<int>(101, 0);
        
        int n;
        cin >> n;
        
        for(int i = 0; i < n; ++i){
            int tmp;
            cin >> tmp;
            ++counter[tmp];
        }
        
        int i;
        for(i = 0; i <= 100 && counter[i] > 0; ++i){
            //used for set A
            --counter[i];
        }
        
        int a = i;
        
        for(i = 0; i <= 100 && counter[i] > 0; ++i){
            --counter[i];
        }
        
        int b = i;
        
        cout << a+b << endl;
    }
 
    return 0;
}
```

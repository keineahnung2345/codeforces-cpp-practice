# simple math

31 ms	3900 KB

time: O(26*S)

```cpp
#include <iostream>
#include <vector>
#include <string>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n;
        cin >> n;
        
        vector<string> strs(n);
        vector<int> counter(26, 0);
        
        for(int i = 0; i < n; ++i){
            cin >> strs[i];
            for(char c : strs[i]){
                ++counter[c-'a'];
            }
        }
        
        bool eq = true;
        for(int i = 0; i < 26; ++i){
            if(counter[i]%n != 0){
                eq = false;
                break;
            }
        }
        
        if(eq) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
 
    return 0;
}
```

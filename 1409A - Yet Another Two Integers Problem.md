140 ms	0 KB

One thing to notice is that we should add `(int)` before `ceil`, otherwise it will return values in such format: `8.76543e+007`.

```cpp
#include <iostream>
#include <cmath>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int a, b;
        cin >> a >> b;
        
        cout << (int)ceil(abs(a-b)/10.0) << endl;
    }
 
    return 0;
}
```

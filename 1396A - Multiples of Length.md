# official solution - math
358 ms	800 KB

There are two cases: when `n == 1` or when `n != 1`.

When `n == 1`, we use one operation to make `a[0]` zero, plus two dummy operations.

When `n != 1`, we:
- make `a[0]` zero
- make `a[i]` become `-(n-1)*a[i]` for i > 0 by `1 n` `0, −n⋅a2, −n⋅a3, …, −n⋅an`(the operation's index is 1-based)
- make `a[i]` become zero for i > 0 by `2 n` `(n−1)⋅a2, (n−1)⋅a3, …, (n−1)⋅an`

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    int n;
    
    cin >> n;
    
    vector<long long> a(n);
    
    for(int i = 0; i < n; ++i){
        cin >> a[i];
    }
    
    if(n == 1){
        cout << "1 1" << endl;
        cout << -a[0] << endl;
        a[0] += -a[0];
        
        //two dummy operations
        cout << "1 1" << endl;
        cout << "0" << endl;
        cout << "1 1" << endl;
        cout << "0" << endl;
    }else{
        //make a[0] 0
        cout << "1 1" << endl;
        cout << -a[0] << endl;
        
        //make a[i] become -(n-1)*a[i]
        cout << "1 " << n << endl;
        cout << "0 ";
        for(int i = 1; i < n; ++i){
            cout << -n*a[i] << " ";
        }
        cout << endl;
        
        //make a[i] 0 for i > 0
        cout << "2 " << n << endl;
        for(int i = 1; i < n; ++i){
            cout << (n-1)*a[i] << " ";
        }
        cout << endl;
    }

    return 0;
}
```

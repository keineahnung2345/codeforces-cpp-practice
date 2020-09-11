# math

217 ms	0 KB

```cpp
#include <iostream>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        long long a, b, x, y, n;
        
        cin >> a >> b >> x >> y >> n;
        
        // cout << "a-x: " << a-x << ", b-y: " << b-y << endl;
        // cout << "n: " << n << endl;
        
        if((a-x >= n) && (b-y >= n)){
            //can just reach the lower bound
            //the lower bound doesn't work
            if(a < b) swap(a,b);
            //operate on the smaller one
            // cout << "both not below lower bound" << endl;
            cout << a * (b-n) << endl;
        }else if(a-x >= n){
            //b-y < n
            // cout << "b below lower bound" << endl;
            if(y <= a-n){
                //b -> y, a -> 
                cout << y * (a-(n-(b-y))) << endl;
            }else{
                //operate on a
                cout << (a-n) * b << endl;
            }
        }else if(b-y >= n){
            // cout << "a below lower bound" << endl;
            if(x <= b-n){
                cout << x * (b-(n-(a-x))) << endl;
            }else{
                cout << (b-n) * a << endl;
            }
        }else{
            // cout << "both below lower bound" << endl;
            //both reach lower bound
            if(n >= (a-x)+(b-y)){
                //n too large
                cout << x*y << endl;
            }else if(x <= y){
                //operate on a
                cout << x * (b-(n-(a-x))) << endl;
            }else{
                cout << y * (a-(n-(b-y))) << endl;
            }
        }
    }
 
    return 0;
}
```

# official solution - math
217 ms	0 KB

```cpp
#include <iostream>
#include <climits>

using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int a, b, x, y, n;
        
        cin >> a >> b >> x >> y >> n;
        
        long long ans = LLONG_MAX;
        for(int i = 0; i < 2; ++i){
            //da, db: actual operations used on a and b
            int da = min(n, a-x);
            //use da operations on a, remaining n-da operations
            int db = min(n-da, b-y);
            
            ans = min(ans, 1LL*(a-da)*(b-db));
            
            swap(a, b);
            swap(x, y);
        }
        
        cout << ans << endl;
    }
 
    return 0;
}
```

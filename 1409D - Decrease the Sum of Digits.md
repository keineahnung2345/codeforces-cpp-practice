# official solution - math
296 ms	0 KB

Iteratively add to make the last non-zero digit of `n` become zero. 

time compleixity: O((log10(n))^2)

```cpp
#include <iostream>
#include <vector>
#include <cmath>
 
using namespace std;

int digitSum(long long n){
    int sum = 0;
    
    while(n){
        sum += n%10;
        n /= 10;
    }
    
    return sum;
}

int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        long long n, s;
        
        cin >> n >> s;
        
        long long moves = 0;
        int dig;
        
        for(int i = 0; i < 18 && digitSum(n) > s; ++i){
            dig = (n/(long long)pow(10LL,i))%10;
            long long move = (10-dig) % 10 * (long long)pow(10LL, i);
            moves += move;
            n += move;
        }
        
        cout << moves << endl;
    }
    
    return 0;
}
```

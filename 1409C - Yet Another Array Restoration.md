# math

46 ms	0 KB

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n, x, y;
        vector<int> factors;
        
        cin >> n >> x >> y;
        
        if(n == 2){
            cout << x << " " << y << endl;
        }else{
            if(x > y) swap(x, y);
            //y is larger
            int diff = y-x;
            
            for(int f = 1; f*f <= diff; ++f){
                if(diff%f == 0){
                    factors.push_back(f);
                    if(f*f != diff)
                        factors.push_back(diff/f);
                }
            }
            
            sort(factors.rbegin(), factors.rend());
            
            bool flag = false;
            for(int& f : factors){
                // cout << "factor: " << f << endl;
                int min_ele = y - (n-1)*f;
                // if(min_ele <= 0){
                //     continue;
                // }
                
                if(min_ele > 0 && min_ele <= x){
                    for(int i = 0; i < n; ++i){
                        cout << min_ele+i*f << " ";
                    }
                    cout << endl;
                    flag = true;
                    break;
                }
            }
            
            if(!flag){
                reverse(factors.begin(), factors.end());
                for(int& f : factors){
                    int max_ele = x + f*(n-1);
                    int min_ele;
                    if(max_ele >= y){
                        //min_ele > x
                        //max_ele should be larger than y
                        min_ele = x;
                        while(min_ele - f > 0){
                            min_ele -= f;
                        }
                        
                        for(int i = 0; i < n; ++i){
                            cout << min_ele+i*f << " ";
                        }
                        cout << endl;
                        break;
                    }
                }
            }
            
            
        }
    }
 
    return 0;
}
```

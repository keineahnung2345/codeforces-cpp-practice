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

# official solution - brute force

46 ms	100 KB

Generate all possible sequences, maintain the one containing `x` and `y`, and choose the one whose last element is the smallest. 

time: O(N^3)

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
        
        cin >> n >> x >> y;
        
        vector<int> ans;
        
        for(int start = 1; start <= 50; ++start){
            //d cannot be 0 because x != y
            for(int d = 1; d <= 50; ++d){
                vector<int> cand(n);
                for(int i = 0; i < n; ++i){
                    cand[i] = start + d * i;
                }
                
                if(find(cand.begin(), cand.end(), x) != cand.end() && 
                    find(cand.begin(), cand.end(), y) != cand.end() && 
                    (ans.empty() || cand.back() < ans.back())){
                    ans = cand;
                }
            }
        }
        
        for(int& e : ans){
            cout << e << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

# official solution 2

31 ms	0 KB

`d` should be a factor of `y-x`.

To generate a sequence, start from y and extend to the left and right.

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
        
        cin >> n >> x >> y;
        
        vector<int> ans;
        
        for(int d = 1; d <= y-x; ++d){
            if((y-x)%d != 0) continue;
            vector<int> cand;
            bool xfound = false;
            int cur = y, need = n;
            
            //from y toward 0
            while(cur >= 1 && need > 0){
                cand.push_back(cur);
                xfound |= (cur == x);
                cur -= d;
                --need;
            }
            
            //from y+d toward INF
            cur = y+d;
            while(need > 0){
                cand.push_back(cur);
                cur += d;
                --need;
            }
            
            sort(cand.begin(), cand.end());
            
            if(need == 0 && xfound && 
                (ans.empty() || cand.back() < ans.back())){
                ans = cand;
            }
        }
        
        for(int& e : ans){
            cout << e << " ";
        }
        cout << endl;
    }
    
    return 0;
}
```

# official solution 3 

31 ms	0 KB

Choose the smallest `d` s.t. `d` is a factor of `y-x` and there won't be more than `n` elements between `x` and `y` in the generated sequence.

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
        
        cin >> n >> x >> y;
        
        vector<int> ans;
        int diff = y-x;
        
        for(int d = 1; d <= y-x; ++d){
            if(diff%d != 0) continue;
            /*
            [x, x+d, x+2*d, ..., x+k*d=y]
            there are (diff/d + 1) elements,
            we should choose a d large enough 
            s.t. (diff/d + 1) <= n
            */
            if(diff/d+1 > n) continue;
            
            int k = min((y-1)/d, n-1);
            
            int a0 = y - k*d;
            
            for(int i = 0; i < n; ++i){
                cout << a0 + i*d << " ";
            }
            cout << endl;
            break;
        }
    }
    
    return 0;
}
```

# Math, greedy
343 ms	2200 KB

If there are only 5 elements in the array, we must use them all. Otherwise, we can discard some elements.
If number of positive and negative elements is less than 5, we must use 0, so the result is 0.
Then we greedily create an array of 0 to 5 negative elements and find the one giving maximum product.

```cpp
#include <iostream>
#include <vector>
#include <climits>
#include <algorithm>
#include <numeric>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n;
        cin >> n;
        
        vector<long long> a(n);
        
        for(int i = 0; i < n; ++i){
            cin >> a[i];
        }
        
        if(n == 5){
            cout << accumulate(a.begin(), a.end(), 1LL, multiplies<long long>()) << endl;
        }else{
            vector<long long> pos, neg;
            int zero = 0;
            
            for(int i = 0; i < n; ++i){
                if(a[i] > 0) pos.push_back(a[i]);
                else if(a[i] < 0) neg.push_back(a[i]);
                else ++zero;
            }
            
            if(pos.size() + neg.size() < 5){
                cout << 0 << endl;
            }else{
                //the larger the former
                sort(pos.rbegin(), pos.rend());
                //the smaller the former
                sort(neg.begin(), neg.end());
                
                long long ans = LLONG_MIN;
                
                for(int neg_cnt = 0; neg_cnt <= 5; ++neg_cnt){
                    int pos_cnt = 5-neg_cnt;
                    if(pos.size() >= pos_cnt && neg.size() >= neg_cnt){
                        if(neg_cnt % 2 == 0){
                            ans = max(ans, 
                                accumulate(pos.begin(), pos.begin()+pos_cnt, 1LL, multiplies<long long>()) * 
                                accumulate(neg.begin(), neg.begin()+neg_cnt, 1LL, multiplies<long long>())
                            );
                        }else{
                            ans = max(ans, 
                                accumulate(pos.end()-pos_cnt, pos.end(), 1LL, multiplies<long long>()) * 
                                accumulate(neg.end()-neg_cnt, neg.end(), 1LL, multiplies<long long>())
                            );
                        }
                    }
                }
                
                if(ans < 0 && zero > 0){
                    ans = 0;
                }
                
                cout << ans << endl;
            }
        }
    }
 
    return 0;
}
```

# Brute force(TLE)
Time limit exceeded on pretest 2	1000 ms	0 KB

Generate all permutations of the input array, and exits if we find a permutation that is not the same as input array and gives the same `F(p)`.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n;
        cin >> n;
        
        vector<int> perm(n);
        
        for(int i = 0; i < n; ++i){
            cin >> perm[i];
        }
        
        unordered_map<int, int> fp;
        
        // cout << "fp: ";
        for(int i = 0; i < n-1; ++i){
            ++fp[perm[i] + perm[i+1]];
            // cout << perm[i] + perm[i+1] << " ";
        }
        // cout << endl;
        
        unordered_map<int, int> fp_tmp;
        vector<int> perm_tmp = perm;
        sort(perm_tmp.begin(), perm_tmp.end());
        
        do{
            fp_tmp.clear();
            // cout << "fp_tmp: ";
            for(int i = 0; i < n-1; ++i){
                // cout << perm_tmp[i] + perm_tmp[i+1] << " ";
                ++fp_tmp[perm_tmp[i] + perm_tmp[i+1]];
            }
            // cout << endl;
            
            if(fp_tmp == fp && perm_tmp != perm){
                for(int& e : perm_tmp){
                    cout << e << " ";
                }
                cout << endl;
                break;
            }
            
        }while(next_permutation(perm_tmp.begin(), perm_tmp.end()));
    }
 
    return 0;
}
```

# Reverse the array

124 ms	0 KB

The answer is as simple as just reversing the input array! 
From the formula: `F(p)=sort([p1+p2,p2+p3,…,pn−1+pn])`, we know that we should keep the relative distance between each element.
And if we reverse the permutation, this formula will give us the same result because it use `sort`.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n;
        cin >> n;
        
        vector<int> perm(n);
        
        for(int i = 0; i < n; ++i){
            cin >> perm[i];
        }
        
        reverse(perm.begin(), perm.end());
        
        for(int& e : perm){
            cout << e << " ";
        }
        cout << endl;
    }
 
    return 0;
}
```

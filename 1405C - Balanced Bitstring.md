# backtracking(TLE)
Time limit exceeded on pretest 4
2000 ms	14100 KB

use backtracking to create all possible strings, finally check if it's k-balanced.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>
 
using namespace std;
 
bool check(string& s, int& k, int cur){
    vector<int> counter(2, 0);
    
    for(int i = cur; i < cur+k; ++i){
        if(s[i] == '0'){
            ++counter[0];
        }else{
            ++counter[1];
        }
        // cout << "0: " << counter[0] << ", 1: " << counter[1] << endl;
        if(counter[0] > (k>>1) || counter[1] > (k>>1)){
            // cout << "invalid" << endl;
            return false;
        }
    }
    
    // cout << "valid" << endl;
    return true;
}
 
bool backtrack(string& s, int& k, int cur){
    int n = s.size();
    if(cur == n){
        return check(s, k, cur-k);
    }else{
        if(cur-k >= 0 && !check(s, k, cur-k)){
            // cout << cur << ": early false" << endl;
            return false;
        }
        
        if(s[cur] == '?'){
            char tmp;
            for(char c : {'0', '1'}){
                s[cur] = c;
                // cout << "revise and backtrack" << endl;
                if(backtrack(s, k, cur+1)){
                    // cout << cur << ": true" << endl;
                    return true;
                }
                s[cur] = '?';
            }
        }else{
            // cout << "direct backtrack" << endl;
            if(backtrack(s, k, cur+1)){
                // cout << cur << ": true" << endl;
                return true;
            }
        }
        // cout << cur << ": false" << endl;
        return false;
    }
};
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n, k;
        cin >> n >> k;
        
        string s;
        cin >> s;
        
        if(backtrack(s, k, 0)){
            cout << "YES\n";
        }else{
            cout << "NO\n";
        }
    }
 
    return 0;
}
```


# official solution
109 ms	1200 KB

The problem want to check whether we can convert `s` to a string s.t. every length `k` substring of `s` has equal amount of 0 and 1.

An important observation is that `s[i...i+k-1]` and `s[i+1...i+k]` share k-1 characters in the middle, 
and if we want both substrings having the same amount of 0 and 1, then `s[i]` and `s[i+k]` must be equal. 
So in the algorithm, we want to check if it's possible to make `s[i]`, `s[i+k]`, `s[i+k*2]`, ... all the same for all `i` in [0,k-1]. 
If it's not, then the answer is NO.

Another observation can be derived from the first observation, i.e. `s[i...i+k-1]` is exactly same as `s[i+k...i+2*k-1]`. 
Knowing this, to check whether each substring having equal amount of 0 and 1, 
we can only check `s[0...k-1]` and we know if `s[0...k-1]` satisfy the condition, then all other substrings satisfy it.

So in the algorithm, we keep the count of 0 and 1 in `s[0...k-1]`, and say NO if either 0's count or 1's count is larger than `k/2`.

```cpp
#include <iostream>

using namespace std;

int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n, k;
        string s;
        
        cin >> n >> k;
        cin >> s;
        
        bool valid = true;
        for(int i = 0; i < k; ++i){
            bool exist0 = false, exist1 = false;
            for(int cur = i; cur < n; cur += k){
                if(s[cur] == '0'){
                    exist0 = true;
                    if(exist1){
                        valid = false;
                        break;
                    }
                }else if(s[cur] == '1'){
                    exist1 = true;
                    if(exist0){
                        valid = false;
                        break;
                    }
                }
            }
            if(exist0){
                s[i] = '0';
            }else if(exist1){
                s[i] = '1';
            }
            if(!valid){
                break;
            }
        }
        
        if(!valid){
            cout << "NO" << endl;
        }else{
            int zeros = 0, ones = 0;
            for(int i = 0; i < k; ++i){
                zeros += (s[i] == '0');
                ones += (s[i] == '1');
            }
            
            if(zeros > k>>1 || ones > k >> 1){
                cout << "NO" << endl;
            }else{
                cout << "YES" << endl;
            }
        }
    }

    return 0;
}

```

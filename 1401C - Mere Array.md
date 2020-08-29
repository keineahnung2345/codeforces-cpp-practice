## backtracking

Memory limit exceeded on test 2	265 ms	262144 KB

Use backtracking to swap the valid pairs, and if the resulting sequence is increasing, return true.

But there is a problem in the code below, to allow a[start] be exchanged for multiple times, when go to the next recursion.
It uses `backtrack(a, amin, start, exc);` rather than `backtrack(a, amin, start+1, exc);`, which could be the reason of MLE.
Need to find a method using `backtrack(a, amin, start+1, exc);` but allowing `a[start]` to be exchanged multiple times.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
 
using namespace std;
 
int n;
 
bool isIncreasing(vector<int>& a){
    // int n = a.size();
    
    for(int i = 1; i < n; ++i){
        if(a[i-1] > a[i]){
            return false;
        }
    }
    
    return true;
}
 
int gcd(int x, int y){
    if(x < y) swap(x, y);
    if(y == 0) return x;
    return gcd(y, x%y);
}
 
bool backtrack(vector<int>& a, int& amin, int start, int last_exc){
    if(isIncreasing(a)){
        return true;
    }else{
        // int n = a.size();
        //excahnge start with something in [start+1...]
        for(int exc = start+1; exc < n; ++exc){
            if(exc != last_exc && gcd(a[start], a[exc]) == amin){
                // cout << "swap a[" << start << "]=" << a[start] << " and a[" << exc << "]=" << a[exc] << endl;
                swap(a[start], a[exc]);
                bool res = backtrack(a, amin, start, exc);
                if(res) return true;
                swap(a[start], a[exc]);
            }
        }
        return false;
    }
}
 
int main()
{
    int t;
    
    cin >> t;
    
    vector<int> a(100000);
    
    while(t-- > 0){
        // int n;
        cin >> n;
        
        // vector<int> a(n);
        
        for(int i = 0; i < n; ++i){
            cin >> a[i];
        }
        
        int amin = *min_element(a.begin(), a.begin()+n);
        bool res = backtrack(a, amin, 0, -1);
        if(res) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
 
    return 0;
}
```

## sorting
217 ms	2300 KB

For the elements in a which are a's minimum's multiple, they can be exchanged arbitrarily.
Suppose a's minimum is a[k], and there are a[i] and a[j] which are a[k]'s multiple, we can exchange i and k, and then k and j, and then j and i, this has the same effect of just swapping a[i] and a[j].

That means we can sort a's minimum's multiple. Following code simulate the process of sort a's minimum's multiple and check if the resulting sequence is increasing.

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
 
using namespace std;
 
bool isIncreasing(vector<int>& a){
    int n = a.size();
    
    for(int i = 1; i < n; ++i){
        if(a[i-1] > a[i]){
            return false;
        }
    }
    
    return true;
}

int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int n;
        cin >> n;
        
        vector<int> a(n);
        
        for(int i = 0; i < n; ++i){
            cin >> a[i];
        }
        
        int amin = *min_element(a.begin(), a.begin()+n);
        
        //exchangable indices
        vector<int> idxs, vals;
        
        for(int i = 0; i < n; ++i){
            if(a[i] % amin == 0){
                idxs.push_back(i);
                vals.push_back(a[i]);
            }
        }
        
        sort(vals.begin(), vals.end());
        
        for(int i = 0; i < idxs.size(); ++i){
            a[idxs[i]] = vals[i];
        }
        
        if(isIncreasing(a)) cout << "YES" << endl;
        else cout << "NO" << endl;
    }
 
    return 0;
}
```

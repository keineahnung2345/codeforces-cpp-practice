This problem gives us an input array `a`, and it requires us to reorder it to create the array `b` so that array `c` is lexicographically maximal.
The definition of array `c` is `ci=GCD(b0,…,bi)`.

# maintain cumulative gcd array
The largest element in `a` gives us the largest gcd, so the first element of `c` must be the largest element in `a`. 
We keep an array `gcds` containing the cumulative gcd, then in each iteration, we choose the element in `a`(which is `ai`) that will maximize `ci`.

31 ms	200 KB

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
 
using namespace std;
 
int main()
{
    int t, n;
    vector<int> A(1000);
    vector<int> gcds(1000);
    
    cin >> t;
    
    while(t-- > 0){
        cin >> n;
        
        for(int i = 0; i < n; ++i){
            cin >> A[i];
            gcds[i] = A[i];
        }
        
        int cur_gcd = *max_element(A.begin(), A.begin()+n);
        int tmp = n;
        while(tmp-- > 0){
            for(int i = 0; i < n; ++i){
                if(gcds[i] != -1){
                    gcds[i] = __gcd(gcds[i], cur_gcd);
                }
            }
            
            auto it = max_element(gcds.begin(), gcds.begin()+n);
            
            int idx = (it-gcds.begin());
            
            cur_gcd = __gcd(cur_gcd, gcds[idx]);
            
            cout << A[idx] << " ";
            
            gcds[idx] = -1;
        }
        
        cout << endl;
    }
 
    return 0;
}
```

# offical solution - O(n^2 * log(max(a)))
31 ms	0 KB

We will do `n` iterations, in `i`th iteration, we calculate gcd of our cumulative gcd(`cumgcd`) with all the remaining `n-i` elements in the array.
We choose the element giving the maximum resulting gcd and remove it from the array.

The time complexity of `gcd(x)` is `O(log(x))`, so the total time complexity is `O(n^2*log(max(a)))`, where `max(a)` is the maximum of the array `a`.
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
 
using namespace std;
 
int main()
{
    int t, n;
    
    cin >> t;
    
    while(t-- > 0){
        cin >> n;
        
        vector<int> a(n);
        for(int i = 0; i < n; ++i){
            cin >> a[i];
        }
        
        int cumgcd = 0;
        
        for(int i = 0; i < n; ++i){
            int curmaxgcd = 0;
            int curmaxidx = -1;
            for(int j = 0; j < n-i; ++j){
                if(__gcd(cumgcd, a[j]) > curmaxgcd){
                    curmaxgcd = __gcd(cumgcd, a[j]);
                    curmaxidx = j;
                }
            }
            
            cout << a[curmaxidx] << " ";
            a.erase(a.begin()+curmaxidx);
            cumgcd = curmaxgcd;
        }
        
        cout << endl;
    }
 
    return 0;
}
```

# official solution - O(len(set(a))^2 * log(max(a)))
31 ms	0 KB

Here we use a maintain a counter of the original array and operate with it. 

The time complexity becomes `the count of unique elements in a ^ 2 * log(max(a))`. In the editorial, the time complexity is `O(AnlogA)`, which is a little weird, post a question [here](https://codeforces.com/blog/entry/82417?#comment-694020).

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>
 
using namespace std;
 
int main()
{
    int t, n;
    
    cin >> t;
    
    while(t-- > 0){
        cin >> n;
        
        unordered_map<int, int> counter;
        for(int i = 0; i < n; ++i){
            int v;
            cin >> v;
            ++counter[v];
        }
        
        int cumgcd = 0;
        
        while(!counter.empty()){
            int curmaxgcd = 0;
            unordered_map<int,int>::iterator curmaxit;
            for(auto it = counter.begin(); it != counter.end(); ++it){
                if(__gcd(cumgcd, it->first) > curmaxgcd){
                    curmaxgcd = __gcd(cumgcd, it->first);
                    curmaxit = it;
                }
            }
            
            for(int i = 0; i < curmaxit->second; ++i){
                cout << curmaxit->first << " ";
            }
            cumgcd = curmaxgcd;
            counter.erase(curmaxit);
        }
        cout << endl;
    }
 
    return 0;
}
```

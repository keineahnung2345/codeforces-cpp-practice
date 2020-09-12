# official solution - 3 dimension DP
62 ms	34100 KB

time: O(N^3)

The state is defined as (chars considered, moves used, count of t0). In each state, we either don't change ith char, change it to `t[0]` or change it to `t[1]`.

```cpp
#include <iostream>
#include <vector>
#include <climits>

using namespace std;

int main()
{
    int n, k;
    string s, t;
    
    cin >> n >> k;
    cin >> s >> t;
    
    //first dimension: consider how many chars, so range is [0,n]
    vector<vector<vector<int>>> dp(n+1, vector<vector<int>>(k+1, vector<int>(n+1, INT_MIN)));

    dp[0][0][0] = 0;

    //i: consider s[i]
    for(int i = 0; i < n; ++i){
        //change times: [0,k]
        for(int ck = 0; ck <= k; ++ck){
            for(int cnt0 = 0; cnt0 <= n; ++cnt0){
                if(dp[i][ck][cnt0] == INT_MIN) continue;
                bool e0 = (s[i] == t[0]);
                bool e1 = (s[i] == t[1]);
                bool e01 = (t[0] == t[1]);
                
                //don't change ith char
                dp[i+1][ck][cnt0+e0] = max(dp[i+1][ck][cnt0+e0], 
                    //if s[i] is t[1], we have cnt0 more sebsequence t
                    dp[i][ck][cnt0] + (e1 ? cnt0 : 0));
                    
                if(ck < k){
                    //change ith char to cnt0
                    dp[i+1][ck+1][cnt0+1] = max(dp[i+1][ck+1][cnt0+1],
                        //s[i] is converted to t[0], if t[0] == t[1],
                        //we have cnt0 more subsequence t
                        dp[i][ck][cnt0] + (e01 ? cnt0 : 0));
                        
                    //change ith char to cnt1
                    dp[i+1][ck+1][cnt0+e01] = max(dp[i+1][ck+1][cnt0+e01],
                        dp[i][ck][cnt0] + cnt0);
                }
            }
        }
    }
    
    int ans = 0;
    for(int ck = 0; ck <= k; ++ck){
        for(int cnt0 = 0; cnt0 <= n; ++cnt0){
            ans = max(ans, dp[n][ck][cnt0]);
        }
    }
    
    cout << ans << endl;

    return 0;
}
```

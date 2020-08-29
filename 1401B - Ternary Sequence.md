## create sequences

Memory limit exceeded on test 3, 265 ms, 262144 KB

To get optimal maximum sum of c, 1st seq's 2 should be paired with 2nd seq's 1(result in 2), less better 2(to consume the 2 in 2nd seq), the worst 0.
1st seq's 1 should be paired with 1st seq's 1 or 0(result in 0), the worst 2(result in -2).
For 1st seq's 0, whatever they are paired with, they give same result = 0.

This method actually create the sequences but it gives MLE.

```cpp
#include <iostream>
#include <vector>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int x1, y1, z1;
        int x2, y2, z2;
        cin >> x1 >> y1 >> z1;
        cin >> x2 >> y2 >> z2;
        
        int n = x1+y1+z1;
        
        // cout << "n: " << n << endl;
        
        vector<int> v1(n);
        fill(v1.begin(), v1.begin()+z1, 2);
        fill(v1.begin()+z1, v1.begin()+z1+y1, 1);
        
        vector<int> v2(n);
        fill(v2.begin(), v2.begin()+y2, 1);
        fill(v2.begin()+y2, v2.begin()+y2+x2, 0);
        fill(v2.begin()+y2+x2, v2.end(), 2);
        
        /*
        2 -> 1, optimal
        2 -> 2, better than 2 -> 0, because it will consume v2's 2
        1 -> 1 or 0 is the same, 2 is the worst
        0 -> all the same
        */
        
        int p21 = min(z1, y2);
        fill(v1.begin(), v1.begin()+p21, 2);
        fill(v2.begin(), v2.begin()+p21, 1);
        int cur = p21;
        z1 -= p21;
        y2 -= p21;
        
        if(z1 > 0){
            // 2 -> 2
            int p22 = min(z1, z2);
            fill(v1.begin()+cur, v1.begin()+cur+p22, 2);
            fill(v2.begin()+cur, v2.begin()+cur+p22, 2);
            z1 -= p22;
            z2 -= p22;
            cur += p22;
        }
        
        if(z1 > 0){
            // 2 -> 0
            int p20 = min(z1, x2);
            fill(v1.begin()+cur, v1.begin()+cur+p20, 2);
            fill(v2.begin()+cur, v2.begin()+cur+p20, 0);
            z1 -= p20;
            x2 -= p20;
            cur += p20;
        }
        
        int p10 = min(y1, x2);
        fill(v1.begin()+cur, v1.begin()+cur+p10, 1);
        fill(v2.begin()+cur, v2.begin()+cur+p10, 0);
        y1 -= p10;
        x2 -= p10;
        cur += p10;
        
        int p11 = min(y1, y2);
        fill(v1.begin()+cur, v1.begin()+cur+p11, 1);
        fill(v2.begin()+cur, v2.begin()+cur+p11, 1);
        y1 -= p11;
        y2 -= p11;
        cur += p11;
        
        int p12 = min(y1, z2);
        fill(v1.begin()+cur, v1.begin()+cur+p12, 1);
        fill(v2.begin()+cur, v2.begin()+cur+p12, 2);
        y1 -= p12;
        z2 -= p12;
        cur += p12;
        
        //0 -> ?
        fill(v1.begin()+cur, v1.end(), 0);
        fill(v2.begin()+cur, v2.end(), 0);
        
        
        // for(int i = 0; i < n; ++i){
        //     cout << v1[i] << " ";
        // }
        // cout << endl;
        
        // for(int i = 0; i < n; ++i){
        //     cout << v2[i] << " ";
        // }
        // cout << endl;
        
        int ans = 0;
        for(int i = 0; i < n; ++i){
            if(v1[i] > v2[i]){
                ans += v1[i]*v2[i];
            }else if(v1[i] < v2[i]){
                ans -= v1[i]*v2[i];
            }
        }
        cout << ans << endl;
    }
 
    return 0;
}
```

## not create sequences
140 ms	4 KB

This method calculate the sum of c on the fly.

```cpp
#include <iostream>
#include <vector>
 
using namespace std;
 
int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int x1, y1, z1;
        int x2, y2, z2;
        cin >> x1 >> y1 >> z1;
        cin >> x2 >> y2 >> z2;
        
        int n = x1+y1+z1;
        
        // cout << "n: " << n << endl;
        
        int ans = 0;
        
        int p21 = min(z1, y2);
        int cur = p21;
        z1 -= p21;
        y2 -= p21;
        // cout << p21 << "(2,1)" << endl;
        ans += 2*p21;
        
        // 2 -> 2
        int p22 = min(z1, z2);
        z1 -= p22;
        z2 -= p22;
        cur += p22;
        // cout << p22 << "(2,2)" << endl;
        
        // 2 -> 0
        int p20 = min(z1, x2);
        z1 -= p20;
        x2 -= p20;
        cur += p20;
        // cout << p20 << "(2,0)" << endl;
        
        int p10 = min(y1, x2);
        y1 -= p10;
        x2 -= p10;
        cur += p10;
        // cout << p10 << "(1,0)" << endl;
        
        int p11 = min(y1, y2);
        y1 -= p11;
        y2 -= p11;
        cur += p11;
        // cout << p11 << "(1,1)" << endl;
        
        int p12 = min(y1, z2);
        y1 -= p12;
        z2 -= p12;
        cur += p12;
        // cout << p12 << "(1,2)" << endl;
        ans -= 2*p12;
        
        //0 -> ?
        // fill(v1.begin()+cur, v1.end(), 0);
        // fill(v2.begin()+cur, v2.end(), 0);
        
        cout << ans << endl;
    }
 
    return 0;
}
```

## official solution
155 ms	0 KB

time: O(1)

The seq c's values can only be in {-2,0,2}. 2 comes from the pair (2,1), and -2 comes from the pair(1,2). So we should create as many (2,1) as possible and as less (1,2) as possible. To create less (1,2), we create many (1,0) and (0,2) to consume seq a's 1 and seq b's 2.

```cpp
#include <iostream>
#include <vector>

using namespace std;

int main()
{
    int t;
    
    cin >> t;
    
    while(t-- > 0){
        int x0, x1, x2;
        int y0, y1, y2;
        
        cin >> x0 >> x1 >> x2 >> y0 >> y1 >> y2;
        
        int ans = 0;
        
        //as more (2,1) as possible
        int m = min(x2, y1);
        x2 -= m;
        y1 -= m;
        ans += 2*m;
        
        //as less (1,2) as possible
        //so build as many (1,0) and (0,2) as possible
        m = min(x1, y0);
        x1 -= m;
        y0 -= m;
        
        m = min(x0, y2);
        x0 -= m;
        y2 -= m;
        
        m = min(x1, y2);
        x1 -= m;
        y2 -= m;
        ans -= 2*m;
        
        cout << ans << endl;
    }

    return 0;
}
```

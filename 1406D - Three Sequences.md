# official solution - math, greedy, difference sequence
936 ms	1200 KB

time: O(n+q)

The problem requires us to construct two sequences `b` and `c`, in which `b` is non-decreasing and `c` is non-increasing and `a[i] = b[i]+c[i]`.
And `max(b[i], c[i])` should be minimized. 
From the monotonic property of `b` and `c`, we know that `b[n-1]` is the maximum in `b`, and `c[0]` is the maximum in `c`.
So our goal is to minimize `max(b[n-1], c[0])`.

Assume `a1 > a0` and we already know `b0`, what is the optimal value of `b1`? 
We know that `b1 = a1 - c1`, and `c1 <= c0`.
In optimal situation, `c1 = c0`(`c` is non-increasing, so it's good to make later element as large as possible).
So in optimal situation, `b1 = a1 - c1 = a1 - c0 = a1 - (a0 - b0) = b0 + (a1 - a0)`. 
It says that when `a[i] > a[i-1]`, the optimal `b[i]` is `b[i-1] + a[i] - a[i-1]` and `c[i]` is same `c[i-1]`.

If `a1 < a0`, `c1 = a1 - b1` and `b1 >= b0`.
In optimal situation, `b1 = b0`, so `c1 = a1 - b1 = a1 - b0 = a1 - (a0 - c0) = c0 + (a1 - a0)`.
So when `a[i] < a[i-1]`, the optimal `c[i]` is `c[i-1] + a[i] - a[i-1]` and `b[i]` is `b[i-1]`.

Let `K` be `sigma(max(0, a[i]-a[i-1]))`, `i` from `1` to `n-1`. 
From the theorem above, we know that `b[n-1] = b[0] + K = a[0] - c[0] + K`, so our goal is to minimize `max(a[0] - c[0] + K, c[0])`.
To reach our goal, we set `a[0] - c[0] + K = c[0]`, so `c[0] = (a[0] + K)/2`.

Here, we have two choices, either make `c[0] = (ceil)((a[0] + K)/2.0)` or `c[0] = (floor)((a[0] + K)/2.0)`. 
Recall that the answer we want to return is `max(b[n-1], c[0])`. 
To simplify our life, we want to ensure `c[0] >= b[n-1]` and just return `c[0]`, i.e. `c[0] >= a[0] - c[0] + K`.
To do this, we simply make `c[0] = (ceil)((a[0] + K)/2.0)`.

To get the answer, we need to maintain `a[0] and K`, and `K` is calculated from the differences of neighboring elements in `a`. 
We will maintain a `diff` array, in which `diff[i] = a[i] - a[i-1]`. 
In each query, we update `a[0]`(if necessary), `diff[l]`(because a[l] is changed and a[l-1] is not changed, so diff[l] must be changed) and `diff[r+1]`,
and then we calculate new `K` from the new `diff`, finally we get our answer.

Note: `a0`, `diff` and `K` initially fit in `int`, but they will be updated, so they have to be `long long`.

Ref: [1406D - Three Sequences(化简贪心+差分)](https://blog.csdn.net/jziwjxjd/article/details/108558775)

```cpp
#include <vector>
#include <iostream>
#include <cmath>

using namespace std;

void update(vector<long long>& diff, long long& K, int pos, int val){
    //diff[0] is meaningless
    if(pos == 0 || pos >= diff.size()) return;
    //if diff[pos] > 0, then it is contained in K
    if(diff[pos] > 0) K -= diff[pos];
    //update the diff: a[pos] - a[pos-1]
    diff[pos] += val;
    //if diff[pos] > 0, need to add it to K
    if(diff[pos] > 0) K += diff[pos];
};

long long getAns(long long& a0, long long& K){
    /*
    b is increasing, c is decreasing
    we want to minimize max(b_n-1, c0),
    
    when a[i]-a[i-1](=diff) > 0,
    we will add diff to b[i-1],
    otherwise we will add diff to c[i-1],
    this is to keep b and c monotonic
    
    b_n-1 = b0 + K
    and 
    b0 = a0 - c0,
    so max(b_n-1, c0) becomes max(a0-c0+K, c0),
    and we want to minimize it,
    let a0-c0+K = c0, so c0 = (a0+K)/2
    */
    return (ceil)((a0 + K)/2.0);
};

int main(){
    int n;
    
    cin >> n;
    
    vector<int> a(n);
    
    for(int i = 0; i < n; ++i){
        cin >> a[i];
    }
    
    //diff and K need to be long long because it will be updated!!
    vector<long long> diff(n);
    long long K = 0;
    for(int i = 1; i < n; ++i){
        diff[i] = a[i] - a[i-1];
        K += max(0LL, diff[i]);
    }
    
    //a0 needs to be long long because it will be updated!!
    long long a0 = a[0];
    cout << getAns(a0, K) << endl;
    
    int q;
    
    cin >> q;
    
    for(int i = 0; i < q; ++i){
        int l, r, x;
        cin >> l >> r >> x;
        //index: 1-based to 0-based
        --l;
        --r;
        //in each iteration we only need to update a0, diff and K
        if(l == 0) a0 += x;
        
        //a[l]-a[l-1](=diff[l]) is changed by x
        update(diff, K, l, x);
        //a[r+1]-a[r](=diff[r+1]) is changed by -x
        update(diff, K, r+1, -x);
        
        cout << getAns(a0, K) << endl;
    }
    
    
    return 0;
}
```

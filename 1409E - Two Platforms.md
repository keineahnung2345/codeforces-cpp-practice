# official solution - sorting, two pointers, dp

Note that we don't need `y` coordinate in this problem.

First sort the points by their `x` coordinate. 
And then calculate the array `l`, in which `l[i]` is the number of points in `[l[i]-k, l[i]]`, also calculate the array `r`. 

We know that we can save more points if the two platforms are not overlapping. 
And also the endpoint of each platform should be at some input point. 
So we will find a `i` s.t. a platform ends before or at `xs[i]` and another starts after or at `xs[i+1]`. 

To do this, we maintain two arrays: `prefix_max_l` and `suffix_max_r`, 
so we can find for the number of points saved for each split place(`xs[i]` and `xs[i+1]`) as fast as possible.

```cpp
#include <iostream>
#include <vector>
#include <cstdio>
#include <algorithm>

using namespace std;

int main() {
#ifdef _DEBUG
	freopen("input.txt", "r", stdin);
	//	freopen("output.txt", "w", stdout);
#endif
	int t;

	cin >> t;

	while (t-- > 0) {
		int n, k;

		cin >> n >> k;

		vector<int> xs(n);

		for (int i = 0; i < n; ++i) {
			cin >> xs[i];
		}

		for (int i = 0; i < n; ++i) {
			int tmp;
			cin >> tmp;
		}

		sort(xs.begin(), xs.end());

		/*
		l[i]: #points in [xs[i]-k, xs[i]]
		right boundary at l[i], 
		how many points are in the range [xs[i]-k, xs[i]]
		*/
		//r[i]: #points in [xs[i], xs[i]+k]
		vector<int> l(n), r(n);
		/*
		prefix maximum(within range of k) of l
		if we put a platform in the range (-INF,l[i]], 
		what is the maximum points it can cover
		*/
		//suffix maximum(within range of k) or r
		vector<int> prefix_max_l(n), suffix_max_r(n);

		for (int fast = 0, slow = 0; fast < n; ++fast) {
			/*
			the check: "slow < n" is unnecessary,
			because in the worst case slow will stop at fast
			*/
			while (/*slow < n && */xs[fast] - xs[slow] > k)
				++slow;

			l[fast] = fast - slow + 1;
			prefix_max_l[fast] = max(l[fast], 
				(fast > 0) ? prefix_max_l[fast - 1] : 0);
		}

		for (int fast = n - 1, slow = n - 1; fast >= 0; --fast) {
			while (xs[slow] - xs[fast] > k)
				--slow;
			
			r[fast] = slow - fast + 1;
			suffix_max_r[fast] = max(r[fast],
				(fast + 1 < n) ? suffix_max_r[fast + 1] : 0);
		}

		int ans = 0;
		for (int i = -1; i < n; ++i) {
			ans = max(ans,
				(i >= 0 ? prefix_max_l[i] : 0) +
				((i + 1 < n) ? suffix_max_r[i + 1] : 0));
		}

		//cout << "l" << endl;
		//for (int i = 0; i < n; ++i) {
		//	cout << l[i] << " ";
		//}
		//cout << endl;

		//cout << "r" << endl;
		//for (int i = 0; i < n; ++i) {
		//	cout << r[i] << " ";
		//}
		//cout << endl;

		//cout << "prefix_max_l" << endl;
		//for (int i = 0; i < n; ++i) {
		//	cout << prefix_max_l[i] << " ";
		//}
		//cout << endl;

		//cout << "suffix_max_r" << endl;
		//for (int i = 0; i < n; ++i) {
		//	cout << suffix_max_r[i] << " ";
		//}
		//cout << endl;

		cout << ans << endl;
	}
	return 0;
}
```

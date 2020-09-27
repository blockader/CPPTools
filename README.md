# Portable Linear Programming Solver in C++
## Introduction
This is a linear programming solver using the simplex algorithm. I have implemented not only the algorithm but also a user-friendly interface.
## Usage
[LeetCode 1601 Maximum Number of Achievable Transfer Requests](https://leetcode.com/problems/maximum-number-of-achievable-transfer-requests/
```cpp
class Solution {
  public:
    int maximumRequests(int n, vector<vector<int>> &requests) {
        int m = requests.size();
        vector<linear_programming::variable<>> v(m);
        vector<linear_programming::constraint<>> c;
        vector<linear_programming::expression<>> e(n);
        linear_programming::expression<> t;
        for (int i = 0; i < m; ++i)
            c.push_back(v[i] >= 0), c.push_back(v[i] <= 1), t += v[i],
                e[requests[i][0]] += v[i], e[requests[i][1]] -= v[i];
        for (int i = 0; i < n; ++i)
            c.push_back(e[i] >= 0), c.push_back(e[i] <= 0);
        return round(linear_programming::maximize(t, c).value());
    }
};
```
# lru_cache(maxsize=None) in C++
## Introduction
Please note this is actually not a LRU cache. This code implements the behavior of Python's functools.lru_cache when the maxsize parameter is None. So, the name comes from Python. One way to implement a dynamic programming algorithm is through memoization and DFS. For Pythoners, a lru_cache(maxsize=None) function decorator will automatically help you to reuse previously calculated values. Can we implement this cute decorator in C++? The answer is yes! I will introduce my impementation.
## Implementation
So, I think in C++ this decorator should be an object that can be called to construct a callable object from our DFS function. We should also be able to provide some additional hints to allow the decorator to use arrays instead of hash tables when possible to make it widely useful. So, please see the [code](https://github.com/blockader/CPPTools/blob/master/lru_cache.cpp) for my implemenation. This may not be an opitmal way. So, feel free to contact me if you have any thought on this.
## Usage
[LeetCode 1553 Minimum Number of Days to Eat N Oranges](https://leetcode.com/problems/minimum-number-of-days-to-eat-n-oranges/)
```cpp
class Solution {
  public:
    int minDays(int n) {
        return lru_cache::sparse()([](auto self, int n) -> int {
            return n ? min(self(n / 2) + n % 2 + 1, self(n / 3) + n % 3 + 1)
                     : -1;
        })(n);
    }
};
```
[LeetCode 1547 Minimum Cost to Cut a Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/)
```cpp
class Solution {
  public:
    int minCost(int n, vector<int> &cuts) {
        cuts.push_back(0);
        cuts.push_back(n);
        sort(begin(cuts), end(cuts));
        return lru_cache::dense(cuts.size(),
                                cuts.size())([&](auto self, int i, int j) {
            if (j - i + 1 == 2)
                return 0;
            int t = ~0u >> 1;
            for (int k = i + 1; k <= j - 1; ++k)
                t = min(t, self(i, k) + self(k, j) + cuts[j] - cuts[i]);
            return t;
        })(0, cuts.size() - 1);
    }
};
```
# Multidimensional Arrays in C++
## Introduction
This code allows one to create a multidimensional dynamic array based on std::vector.
## Implementation
Please see the [code](https://github.com/blockader/CPPTools/blob/master/vect.cpp) for my implementation.
## Usage
```cpp
// A 2x2x2 integer array with all values initialized to zero is created.
auto a=vect<int>(0,2,2,2);
```

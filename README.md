# lru_cache
## Introduction
One way to implement a dynamic programming algorithm is through memoization and DFS. For Pythoners, a lru_cache(maxsize=None) function decorator will automatically help you to reuse to previously calculated values. Can we implement this cute decorator in C++? The answer is yes! I will introduce my impementation.
## Implementation
So, I think in C++, this decorator should be an object that can be called to construct a callable object from our DFS function. We should also be able to provide some addtional hints to allow the decorator to use arrays instead of hash tables when possible to make it widely useful. So, please see the code in lru_cache.pp for my implemenation. This may not be an opitmal way. So, feel free to contact me if you have any thought on this.
## Usage
```cpp
// https://leetcode.com/problems/minimum-number-of-days-to-eat-n-oranges/
class Solution {
public:
    int minDays(int n) {
        return lru_cache::sparse()([](auto self,int n)->int{
            return n?min(self(n/2)+n%2+1,self(n/3)+n%3+1):-1;
        })(n);
    }
};
// https://leetcode.com/problems/minimum-cost-to-cut-a-stick/
class Solution {
public:
    int minCost(int n, vector<int>& cuts) {
        cuts.push_back(0);
        cuts.push_back(n);
        sort(begin(cuts),end(cuts));
        return lru_cache::dense(cuts.size(),cuts.size())([&](auto self,int i,int j){
            if(j-i+1==2)
                return 0;
            int t=~0u>>1;
            for(int k=i+1;k<=j-1;++k)
                t=min(t,self(i,k)+self(k,j)+cuts[j]-cuts[i]);
            return t;
        })(0,cuts.size()-1);
    }
};
```

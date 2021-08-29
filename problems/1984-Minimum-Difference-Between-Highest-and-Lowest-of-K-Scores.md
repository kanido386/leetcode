## 1984. Minimum Difference Between Highest and Lowest of K Scores (Easy)

[題目連結](https://leetcode.com/problems/minimum-difference-between-highest-and-lowest-of-k-scores/)

本題耗時：14分36秒（有一次嘗試錯誤）

### 我的解題思路

排序 &rarr; sliding window

### C++

```cpp
// 2021-08-29 星期日
// Runtime: 15 ms
// Memory Usage: 13.6 MB

class Solution {
public:
    int minimumDifference(vector<int>& nums, int k) {
        
        if (k == 1) {
            return 0;
        }
        
        sort(nums.begin(), nums.end());
        
        int minimum = 100000;
        
        for (int i = (k-1); i < nums.size(); ++i) {
            if (nums[i] - nums[i-(k-1)] < minimum) {
                minimum = nums[i] - nums[i-(k-1)];
            }
        }
        
        return minimum;
        
    }
};
```

### 可能對未來解題有幫助

- 可用 `LLONG_MAX` 來代表一個很大很大的數值，很小的則是 `LLONG_MIN`，詳細見 [climits](https://www.cplusplus.com/reference/climits/)
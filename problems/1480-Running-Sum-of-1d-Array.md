## 1480. Running Sum of 1d Array (Easy)

[題目連結](https://leetcode.com/problems/running-sum-of-1d-array/)

### 為何會想做這一題？
有好一段時間沒用 C++ 解題了，找個沒什麼難度的題目來抓感覺（而且 LeetCode 提供的 default code 是用 `class` 來包 Solution，有點陌生）

### C++

```cpp
// 2021-08-24 星期二
// Runtime: 12 ms (7.45%)
// Memory Usage: 8.5 MB (34.21%)

class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        int n = nums.size();
        vector<int> result (nums);
        for (int i = 0; i < n - 1; ++i) {
            for (int j = i + 1; j < n; ++j) {
                result[j] += nums[i];
            }
        }
        return result;
    }
};
```

做完看了 Solution 以後，發現有更聰明的作法，找時間再來做吧！

### 學到了什麼？
不要一找到了解決辦法就認為它是最佳解，或許再多花一點時間，就能想出更簡單、更有效率的方法。（不僅限於程式方面，人生其他事情一樣能套用這個道理）
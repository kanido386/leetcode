## 1929. Concatenation of Array (Easy)

[題目連結](https://leetcode.com/problems/concatenation-of-array/)

### C++

```cpp
// 2021-08-25 星期三
// Runtime: 8 ms (84.86%)
// Memory Usage: 12.5 MB (66.46%)

class Solution {
public:
    vector<int> getConcatenation(vector<int>& nums) {
        int n = nums.size();
        vector<int> ans (n * 2);
        for (int i = 0; i < n; ++i) {
            ans[i] = ans[i + n] = nums[i];
        }
        return ans;
    }
};
```

### 其他人的做法

1. 直接 `nums.push_back(nums[i])`，都忘記 `vector` 可以在不手動增加它 size 的情況下，在它最後面加上新 element 了
2. 用 `vector` 的 `insert()`（it takes O(n) time）
    ```cpp
    // void insert (iterator position, InputIterator first, InputIterator last);
    nums.insert(nums.end(), nums.begin(), nums.end());
    ```
3. 用一個從沒聽說過的 `copy_n()`，感覺蠻酷的
    ```cpp
    // OutputIterator copy_n (InputIterator first, Size n, OutputIterator result);
    vector<int> getConcatenation(vector<int>& nums) {
        int len = nums.size();
        vector<int> res(len * 2);
        copy_n(nums.begin(), len, res.begin());
        copy_n(nums.begin(), len, res.begin() + len);
        return res;
    }
    ```
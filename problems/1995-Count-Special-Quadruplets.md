## 1995. Count Special Quadruplets (Easy)

[題目連結](https://leetcode.com/problems/count-special-quadruplets/)

本題耗時：10分37秒（有一次嘗試錯誤）

### 我的解題思路

這題 `4 <= nums.length <= 50`，很小，所以可以直接暴力解！（`O(n^4)` 也沒問題）

### AC前我錯在哪？

題目說 `a < b < c < d` ，看成要對 `nums` 做排序，不知道當時腦袋在想什麼⋯⋯

### C++

```cpp
// 2021-09-05 星期日
// Runtime: 194 ms (33.33%)
// Memory Usage: 10.4 MB

class Solution {
public:
    int countQuadruplets(vector<int>& nums) {
        
        int n = nums.size();
        int numQuadruplet = 0;
        
        // sort(nums.begin(), nums.end());
        
        // for (int i = 0; i < n; ++i) {
        //     cout << nums[i] << endl;
        // }
        
        for (int a = 0; a < n; ++a) {
            for (int b = a+1; b < n; ++b) {
                for (int c = b+1; c < n; ++c) {
                    for (int d = c+1; d < n; ++d) {
                        if (nums[a] + nums[b] + nums[c] == nums[d]) {
                            numQuadruplet++;
                        }
                    }
                }
            }
        }
        
        return numQuadruplet;
        
    }
};
```

### 其他人的做法

用空間換取時間

[[C++] Time: O(n^3) Space: O(n). Similar to 2sum](https://leetcode.com/problems/count-special-quadruplets/discuss/1445269/C%2B%2B-Time%3A-O(n3)-Space%3A-O(n).-Similar-to-2sum)

```cpp
class Solution {
public:
    int countQuadruplets(vector<int>& nums) {
        const auto n = nums.size();
        unordered_map<int, int> freq;
        
        freq[nums[n - 1]] = 1;
        size_t answ = 0;
        for (int i = n - 2; i > 1; --i)
        {
            for (int j = i - 1; j > 0; --j)
            {
                for (int k = j - 1; k >= 0; --k)
                {
                    if (freq.count(nums[i] + nums[j] + nums[k]))
                    {
                        answ += freq[nums[i] + nums[j] + nums[k]];
                    }
                }
            }
            freq[nums[i]] += 1;
        }
        return answ;
    }
};
```
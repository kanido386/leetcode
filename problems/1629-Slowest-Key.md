## 1629. Slowest Key (Easy)

[題目連結](https://leetcode.com/problems/slowest-key/)

本題耗時：7分39秒

### 我的解題思路

1. 用兩個變數分別存「要 return 的 key」及「目前最長的 duration」
2. 直接一一 traverse，有更長的 duration 就更新（若當前和最大的長度一樣，就比較字母順序）

### C++

```cpp
// 2021-09-07 星期二
// Runtime: 15 ms (13.21%)   每次提交都差不少，試著再提交一次，只花 3 ms (97.98%)
// Memory Usage: 10.7 MB (23.52%)

// Complexity
//   - Time:  O(n)
//   - Space: O(1)

class Solution {
public:
    char slowestKey(vector<int>& releaseTimes, string keysPressed) {
        
        char the_key = keysPressed[0];
        int longestDuration = releaseTimes[0];
        
        for (int i = 1; i < releaseTimes.size(); ++i) {
            int duration = releaseTimes[i] - releaseTimes[i-1];
            if (duration > longestDuration) {
                longestDuration = duration;
                the_key = keysPressed[i];
            } else if (duration == longestDuration) {
                the_key = (the_key > keysPressed[i]) ? the_key : keysPressed[i];
            }
        }
        
        return the_key;
        
    }
};
```
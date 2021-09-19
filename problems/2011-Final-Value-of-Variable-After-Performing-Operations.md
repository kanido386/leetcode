## 2011. Final Value of Variable After Performing Operations (Easy)

[題目連結](https://leetcode.com/problems/final-value-of-variable-after-performing-operations/)

本題耗時：3分24秒

### 我的解題思路

`+` 就加一，`-` 就減一。
（這題基本上沒有難度）

### C++

```cpp
// 2021-09-19 星期日
// Runtime: 8 ms
// Memory Usage: 14.1 MB

class Solution {
public:
    int finalValueAfterOperations(vector<string>& operations) {
        
        int result = 0;
        for (int i = 0; i < operations.size(); ++i) {
            if (operations[i][1] == '+') {
                result++;
            } else {
                result--;
            }
        }
        
        return result;
    }
};
```
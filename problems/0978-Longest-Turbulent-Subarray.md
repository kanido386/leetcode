## 978. Longest Turbulent Subarray (Medium)

[題目連結](https://leetcode.com/problems/longest-turbulent-subarray/)

本題耗時：26分43秒（有 3 次嘗試錯誤）

### 我的解題思路

要滿足 Turbulent Subarray，任一元素要比兩旁來得大 or 來得小，就用一個 for 迴圈去看，若有更長的，就更新 `largest` 變數

### AC前我錯在哪？

- 沒考慮到當所有元素都一樣時的情況（output 要是 1）
- 原本 for 迴圈是 `i < n-1`，為的是不要超過 arr 的 index，但這樣到最後一個元素時，就不會更新 largest 的數值（因為進 `if` 就不會進到 `else`）

### C++

```cpp
// 2021-09-15 星期三
// Runtime: 76 ms (57.08%)
// Memory Usage: 40.4 MB (53.05%)

class Solution {
public:
    int maxTurbulenceSize(vector<int>& arr) {
        
        int n = arr.size();
        
        // 其實 n == 1 是多餘的
        if (n == 1 || isAllTheSame(arr)) {
            return 1;
        } else if (n == 2) {
            return 2;
        }
        
        int largest = 0;
        int cur = 0;
        
        for (int i = 1; i < n; ++i) {
            // 中間是否最大 or 最小
            if (i != n-1 && (arr[i] > arr[i-1] && arr[i] > arr[i+1] || arr[i] < arr[i-1] && arr[i] < arr[i+1])) {
                cur++;
            } else {        // 斷掉了
                cur += 2;   // 頭尾沒算到
                largest = max(largest, cur);
                cur = 0;
            }
        }
        
        return largest;
        
    }
    
    static bool isAllTheSame(vector<int>& arr) {
        bool result = true;
        int number = arr[0];
        for (int i = 1; i < arr.size(); ++i) {
            if (arr[i] != number) {
                result = false;
                break;
            }
        }
        return result;
    }
};
```
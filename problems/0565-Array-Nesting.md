## 565. Array Nesting (Medium)

[題目連結](https://leetcode.com/problems/array-nesting/)

本題耗時：26分10秒（有一次嘗試錯誤）

### 我的解題思路

先嘗試用 brute force 的方法在紙上跑一遍，後來發現可以分群(?)欸！
以題目的 Example 1 為例，0 2 5 6 同一組、1 4 同一組、3 自己一組。
什麼意思？因為用最 naive 的想法一個一個去試，發現同組間無論選哪一個數字當頭，最後都會循環到那個數字，至於是不是巧合我不知道，這只是在模擬過程中觀察到的規律。
所以利用這個規律，我們只需要找到最大的組內個數即可。（用上面的例子來說就是 0 2 5 6 那一組，也就是 4）

### AC前我錯在哪？

我原本判斷是否造訪過的 code 寫在 `for (int i = 0; i < n && !isGrouped[i]; ++i)` 裡面，居然犯了這個愚蠢的邏輯錯誤xD（還以為我想法是錯的，多花了些時間驗證想法）

### C++

```cpp
// 2021-09-02 星期四
// Runtime: 19 ms (66.71%)
// Memory Usage: 30.1 MB (48.80%)

class Solution {
public:
    int arrayNesting(vector<int>& nums) {
        
        int n = nums.size();
        vector<bool> isGrouped(n, false);
        int longest = 0;
        
        for (int i = 0; i < n; ++i) {
            
            if (isGrouped[i]) {
                continue;
            }
            
            int thisGroupLength = 0;
            int firstNumber = i;
            int temp = firstNumber;
            do {
                isGrouped[temp] = true;
                temp = nums[temp];
                thisGroupLength++;
            } while (temp != firstNumber);
            
            if (thisGroupLength > longest) {
                longest = thisGroupLength;
            }
        }
        
        return longest;
        
    }
};
```

### 其他人的做法

- 因為 `1 <= nums.length <= 100000`，再加上造訪過的數字就不會再造訪了，所以其實不需要多花空間來判別有沒有造訪過 (visited，我是用 `isGrouped` 這個 vector 來判斷)，可以用 `nums[i] = 大於10000或小於1的值` 的方式來做，酷斃了！
- 有人用 DFS 來做，想了解的話看 Discuss 吧！
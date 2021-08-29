## 522. Longest Uncommon Subsequence II (Medium)

[題目連結](https://leetcode.com/problems/longest-uncommon-subsequence-ii/)

### 我的解題思路

1. 因為要找最長的，所以先 [sort string array by size](https://stackoverflow.com/questions/45865239/how-do-i-sort-string-of-arrays-based-on-length-then-alphabetic-order)，由長排到短（`MyMoreString` 就是參考那篇寫的，不過疑似有更好記、更簡單的寫法）
2. 從最長的開始，若某 string 跟其他同長度的 string 都不一樣，就回傳該 string 的長度（不過要先跟比較長的 string 比較就是了，即第三點）
3. 長度較短的會跟長度較長的 string 判斷是否有 subsequence 的關係，有的話就代表它不會是 the longest uncommon subsequence，直接換下一個

### 注意

我現在仔細看才注意到，題目說的是 `subsequence` 而不是 `substring`，真的要仔細看題目的敘述！
（`abc` is a subsequence of `aebdc`）

### C++

```cpp
// 2021-08-27 星期五
// Runtime: 4 ms (69.36%)
// Memory Usage: 8.3 MB (63.64%)

class Solution {
public:
    int findLUSlength(vector<string>& strs) {
        
        // sort string array by size
        sort(strs.begin(), strs.end(), MyMoreString{});
        
        for (int i = 0; i < strs.size(); ++i) {
            
            bool isLongestUncommon = true;
            
            for (int j = 0; j < strs.size(); ++j) {
                if (i == j) {
                    continue;
                } else if (strs[i].length() == strs[j].length() && strs[i] == strs[j]) {
                    isLongestUncommon = false;
                    break;
                } else if (strs[i].length() < strs[j].length() && isSubstring(strs[j], strs[i])) {
                    isLongestUncommon = false;
                    break;
                } else if (strs[i].length() > strs[j].length()) {
                    break;
                }
            }
            
            if (isLongestUncommon) {
                return strs[i].length();
            }
        }
        
        return -1;
    }
    
    bool isSubstring(const string& big, const string& small) {
        int nextStart = 0;
        for (int i = 0; i < small.length(); ++i) {
            bool maybeCanSuccess = false;
            for (int j = nextStart; j < big.length(); ++j) {
                if (small[i] == big[j]) {
                    maybeCanSuccess = true;
                    nextStart = j + 1;
                    break;
                }
            }
            if (!maybeCanSuccess) {
                return false;
            }
        }
        return true;
    }
    
    struct MyMoreString {
        // operator overloading
        // lhs = left hand side, rhs = right hand side
        bool operator () (const string& lhs, const string& rhs) const {
            // use 'tie' will have 'compile error'...
            return make_pair(lhs.length(), lhs) > make_pair(rhs.length(), rhs);
        }
    };   // Don't forget ';' after struct
};
```

### 收穫

- 題目要看仔細清楚，不要太「先入為主」。
- 後來寫別題，發現用 `make_pair()` 效率會很差，參考[這題其他人的做法](./1985-Find-the-Kth-Largest-Integer-in-the-Array.md)

### 其他人的做法

[C++ Simple and Clean 4ms Solution, Detailed Explanation](https://leetcode.com/problems/longest-uncommon-subsequence-ii/discuss/1429061/C%2B%2B-Simple-and-Clean-4ms-Solution-Detailed-Explanation)

這個寫法我喜歡：

```cpp
unordered_map<string, int> freq;
for (auto str : strs) freq[str]++;
```


我的 `MyMoreString` 可以改成這樣寫：

```cpp
static bool compare(string& a, string& b) {
    return a.size() > b.size();
}

sort(strs.begin(), strs.end(), compare);
```

而我的 `isSubstring()`（而且根本不是 substring xD）或許可以試著這樣寫：

```cpp
bool isSubsequence(string s, string t) {
    int s_i = 0, t_i = 0;
    
    while (s_i < s.size() && t_i < t.size()) {
        if (s[s_i] == t[t_i++]) s_i++; 
    }
    
    return s_i == s.size();
}
```
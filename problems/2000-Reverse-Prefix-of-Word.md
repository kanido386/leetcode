## 2000. Reverse Prefix of Word (Easy)

[題目連結](https://leetcode.com/problems/reverse-prefix-of-word/)

本題耗時：7分40秒

### 我的解題思路

1. 找 `ch` 的 index
2. 對 index 前的字元做 reverse

### C++

```cpp
// 2021-09-12 星期日
// Runtime: 4 ms (40.00%)
// Memory Usage: 6.2 MB (53.33%)

class Solution {
public:
    string reversePrefix(string word, char ch) {
        
        string result = word;
        int index = -1;
        
        for (int i = 0; i < word.length(); ++i) {
            if (word[i] == ch) {
                index = i;
                break;
            }
        }
        
        for (int i = 0; i <= index; ++i) {
            result[i] = word[index-i];
        }
        
        return result;
        
    }
};
```

### 其他人的做法

[Find and reverse](https://leetcode.com/problems/reverse-prefix-of-word/discuss/1458398/Find-and-reverse/1080660)

結合 `find` 和 `reverse` 的做法，簡潔有力！

```cpp
class Solution {
public:
    string reversePrefix(string& word, const char& ch) {
        reverse(word.begin(), word.begin() + word.find(ch) + 1);
        return word;
    }
};
```
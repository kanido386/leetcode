## 917. Reverse Only Letters (Easy)

[題目連結](https://leetcode.com/problems/reverse-only-letters/)

本題耗時：8分47秒

### 我的解題思路

1. 把字串中的 letter 都替換成 `~` 並將那些 letter 依序存入一個 stack（為了 reverse）
    - `wsg30cba` 會變成 `~~~30~~~`，stack 由上至下為 `abcgsw`
2. 讀新字串，遇到 `~` 就替換成 stack 最上面的 letter
    - `~~~30~~~` 會變成 `abc30gsw`

### C++

```cpp
// 2021-09-15 星期三
// Runtime: 3 ms (10.77%)
// Memory Usage: 6 MB (65.19%)

class Solution {
public:
    string reverseOnlyLetters(string s) {
        
        stack<char> box;
        string result = "";
        char temp;
        
        for (int i = 0; i < s.length(); ++i) {
            if (isLetter(s[i])) {
                box.push(s[i]);
                result += '~';
            } else {
                result += s[i];
            }
        }
        
        for (int i = 0; i < result.length(); ++i) {
            if (result[i] == '~') {
                temp = box.top();
                result[i] = temp;
                box.pop();
            }
        }
        
        return result;
        
    }
    
    static bool isLetter(char c) {
        return c >= 'A' && c <= 'Z' || c >= 'a' && c <= 'z';
    }
};

```

### 其他人的做法

[easy C++ with comments - two pointer based approach](https://leetcode.com/problems/reverse-only-letters/discuss/200878/easy-C%2B%2B-with-comments-two-pointer-based-approach)

reverse 也能用「兩兩交換」的方式來達成

```cpp
string reverseOnlyLetters(string S) {
    for (int i = 0, j = S.length() - 1; i < j;) {
        if (!isalpha(S[i]))
            ++i;
        else if (!isalpha(S[j]))
            --j;
        else
            swap(S[i++], S[j--]);
    }
    return S;
}
```
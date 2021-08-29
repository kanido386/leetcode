## 1985. Find the Kth Largest Integer in the Array

[題目連結](https://leetcode.com/problems/find-the-kth-largest-integer-in-the-array/)

本題耗時：11分13秒

### 我的解題思路

- 本來想把字串轉成數字，但看到 `1 <= nums[i].length <= 100` 就知道這不是本題要的。
- 所以改去想數字的特性(?)，位數比較多的會比位數少的大，若位數相同，則從最大位數的數字一一去做比較。
- 上面的特性套到 string 上，位數多寡不就是 length 的長短嗎？一個位數、一個位數去做比較，不是可以用字元的 ascii 順序比嗎？
- 所以如何排序？先比字串 length，再比它們的 ascii order！

### C++

```cpp
// 2021-08-29 星期日
// Runtime: 1322 ms
// Memory Usage: 335.3 MB

class Solution {
public:
    string kthLargestNumber(vector<string>& nums, int k) {
        
        sort(nums.begin(), nums.end(), compare);
        return nums[k-1];
        
    }
    
    static bool compare(const string& a, const string& b) {
        return make_pair(a.length(), a) > make_pair(b.length(), b);
    }
};
```

### 其他人的做法

有人的 `compare()` 是這麼寫的：

```cpp
static bool compare(string &a,string &b) {
    if (a.size() == b.size()) {
        return a > b;
    }
    return a.size() > b.size();
}
```

剛剛試了一下，發現這樣寫效率好很多欸！（而且又更容易理解）
Runtime 從 1322 ms 提升到 204 ms
Memory 從 335.3 MB 減少為 55.3 MB
該學起來了吧！
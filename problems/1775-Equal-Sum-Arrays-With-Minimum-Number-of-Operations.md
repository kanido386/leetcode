## 1775. Equal Sum Arrays With Minimum Number of Operations (Medium)

[題目連結](https://leetcode.com/problems/equal-sum-arrays-with-minimum-number-of-operations/)

本題耗時：46分02秒

### 我的解題思路

1. 多的 array 要減少、少的 array 要增加
2. 每次分別減少越多、增加越多，總共所需的操作次數就會越少
    1. 多的那一方，先由大排到小，再減去 1，代表可減少的量（index 越前面，量越多）
    2. 少的那一方，先由小排到大，再跟 6 取絕對值，代表可增加的量（index 越前面，量越多）
3. 是的多的那方先減還是少的那方先增不重要，重要的是能改變的量越多就要越優先，因為可以更快讓兩者 equal，所以把第二步驟的那兩個 vector 合併起來，再由大排到小
4. 接下來就是一一去判斷 (當次是否能使 equal) 及改變 (兩者之間的 diff) 了

### C++

```cpp
// 2021-09-06 星期一
// Runtime: 356 ms (7.69%)
// Memory Usage: 121.2 MB (22.88%)

class Solution {
public:
    int minOperations(vector<int>& nums1, vector<int>& nums2) {
        
        int n1 = nums1.size();
        int n2 = nums2.size();
        
        if (n1 > n2 * 6 || n2 > n1 * 6) {
            return -1;
        }
        
        int sum1 = accumulate(nums1.begin(), nums1.end(), 0);
        int sum2 = accumulate(nums2.begin(), nums2.end(), 0);
        int diff = abs(sum1 - sum2);
        
        if (sum1 == sum2) {
            return 0;
        } else if (sum1 > sum2) {
            doSomeMagic(nums1, nums2);
        } else {
            doSomeMagic(nums2, nums1);
        }
        
        vector<int> mergedVector = mergeAndSort(nums1, nums2);
        
        for (int i = 0; i < mergedVector.size(); ++i) {
            if (diff <= mergedVector[i]) {
                return i + 1;
            }
            diff -= mergedVector[i];
        }
        
        return -1;
        
    }
    
    vector<int> mergeAndSort(vector<int>& a, vector<int>& b) {
        vector<int> temp(a);
        for (auto e: b) {
            temp.push_back(e);
        }
        sort(temp.begin(), temp.end(), cmpDescending);
        return temp;
    }
    
    void doSomeMagic(vector<int>& a, vector<int>& b) {
        sort(a.begin(), a.end(), cmpDescending);
        sort(b.begin(), b.end(), cmpAscending);
        for (int i = 0; i < a.size(); ++i) {
            a[i] = a[i] - 1;
        }
        for (int i = 0; i < b.size(); ++i) {
            b[i] = 6 - b[i];
        }
    }
    
    static bool cmpAscending(int a, int b) {
        return a < b;
    }
    
    static bool cmpDescending(int a, int b) {
        return a > b;
    }
};
```

### 其他人的做法

因為就如同前面所說的，誰先增減並不重要，所以那個可以增減的量其實可以存進一個 array 或是 map，代表說那個增減量有幾個可以拿來縮小差距。

```cpp
// map to store the possible increase/decrease values
map<int,int> mp;
for(auto it: nums1) mp[it-1]++;
for(auto it: nums2) mp[6-it]++;
```

### 可能對未來解題有幫助

可以用 `swap()` 來交換兩個 `vector`

```cpp
vector<int> nums1;
vector<int> nums2;
swap(nums1, nums2);  // will just call nums1.swap(nums2)
```
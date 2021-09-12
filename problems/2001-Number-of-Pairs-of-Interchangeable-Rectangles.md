## 2001. Number of Pairs of Interchangeable Rectangles (Medium)

[題目連結](https://leetcode.com/problems/number-of-pairs-of-interchangeable-rectangles/)

本題耗時：11分01秒（有一次嘗試錯誤）

### 我的解題思路

1. 先依照長寬比來分 group
2. 每個 group 兩兩配對，根據高中學的排列組合，若每組 n 個，則會有 `C n 取 2` 種組合，也就是 `n(n-1)/2` 種。

### AC前我錯在哪？

把 `int` 改成 `long long` 就過了⋯⋯

可見 data type 也要注意！

### C++

```cpp
// 2021-09-12 星期日
// Runtime: 569 ms
// Memory Usage: 140.5 MB (71.43%)

class Solution {
public:
    long long interchangeableRectangles(vector<vector<int>>& rectangles) {
        
        unordered_map<double, long long> group;
        double cur;
        long long n;
        long long numPairs = 0;
        
        for (int i = 0; i < rectangles.size(); ++i) {
            cur = (double) rectangles[i][0] / rectangles[i][1];
            if (group.find(cur) == group.end()) {
                group[cur] = 1;
            } else {
                group[cur]++;
            }
        }
        
        for (auto it = group.begin(); it != group.end(); ++it) {
            n = it->second;
            numPairs += (n * (n-1) / 2);
        }
        
        return numPairs;
        
    }
};
```

### 其他人的做法

[Intuition Explained || Map || GCD || C++](https://leetcode.com/problems/number-of-pairs-of-interchangeable-rectangles/discuss/1458508/Intuition-Explained-oror-Map-oror-GCD-oror-C%2B%2B)

其實可以用一個 for loop 搞定，因為每新增一個，就是跟原本組內所有的成員一一配對，直接加上原本組內個數即可

```cpp
long long interchangeableRectangles(vector<vector<int>>& rectangles) 
{
	long long result = 0;
	unordered_map<double, int> mp;

	for (auto& rect : rectangles)
	{
		double ratio = (double)rect[0] / rect[1];
		if(mp.find(ratio) != mp.end()) result += mp[ratio];
		mp[ratio]++;
	}

	return result;
}
```

更準確、更快速的做法：

```cpp
long long interchangeableRectangles(vector<vector<int>>& rectangles) 
{
	long long result = 0;
	map<pair<int, int>, int> mp;

	for (auto& rect : rectangles)
	{
		int gcd = __gcd(rect[0], rect[1]);
		pair<int, int> key = {rect[0]/gcd, rect[1]/gcd};
		if(mp.find(key) != mp.end()) result += mp[key];
		mp[key]++;
	}

	return result;
}
```
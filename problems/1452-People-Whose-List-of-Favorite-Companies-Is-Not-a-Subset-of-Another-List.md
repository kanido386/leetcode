## 1452. People Whose List of Favorite Companies Is Not a Subset of Another List (Medium)

[題目連結](https://leetcode.com/problems/people-whose-list-of-favorite-companies-is-not-a-subset-of-another-list/)

本題耗時：32分47秒

### 我的解題思路

1. 題目說要找出「is not a subset」，但反面的「找出 subset」比較容易，就從後者下手吧
2. 要如何成為別人的 subset？這就要大概想一下 subset 的特性。要成為 subset，元素個數一定要小於等於另一方，所以先照著 list 的個數進行排序，也就是 code 中的 `cmp1`**（後來想想，其實不用先排序也沒關係，一來就不用額外 push_back index，二來下面的判斷方式並不會因為前者個數比較多而有所影響）**
3. 除了元素個數，還有什麼呢？沒錯，另一方都要有 subset 那一方的所有元素！難道要用雙重迴圈一一去判斷嗎？想到上次解[這一題](https://github.com/kanido386/leetcode/blob/master/problems/0522-Longest-Uncommon-Subsequence-II.md#其他人的做法)學到的技巧（`isSubstring()` 那邊），就用那個方法拿來判斷吧！但首先，必須對每個各別 list 的元素進行排序 `cmp2` 才能用那個方法。
4. 接著就可以倆倆判斷前者 (元素個數小於等於後者) 是否為後者的 subset

### C++

```cpp
// 2021-09-07 星期二
// Runtime: 1303 ms (26.16%)
// Memory Usage: 55.2 MB (82.56%)

class Solution {
public:
    vector<int> peopleIndexes(vector<vector<string>>& favoriteCompanies) {
        
        vector<int> theList;
        vector<bool> isNotSubset(favoriteCompanies.size(), true);
        
        // 因為排序後原本的 index 會亂掉，所以要存一下
        for (int i = 0; i < favoriteCompanies.size(); ++i) {
            favoriteCompanies[i].push_back(to_string(i));
        }
        
        // 2.
        sort(favoriteCompanies.begin(), favoriteCompanies.end(), cmp1);

        // 3.
        for (int i = 0; i < favoriteCompanies.size(); ++i) {
            sort(favoriteCompanies[i].begin(), favoriteCompanies[i].end(), cmp2);
        }

        // 4.        
        for (int i = 0; i < favoriteCompanies.size(); ++i) {
            for (int j = i+1; j < favoriteCompanies.size(); ++j) {
                // 3. 提到的技巧
                int a = 1, b = 1;
                while (a < favoriteCompanies[i].size() && b < favoriteCompanies[j].size()) {
                    if (favoriteCompanies[i][a] == favoriteCompanies[j][b++]) {
                        a++;
                    }
                }
                if (a == favoriteCompanies[i].size()) {
                    // 進到這裡代表說是後者的 subset，因為前者的每個 index 都跑完了
                    int index = stoi(favoriteCompanies[i][0]);
                    isNotSubset[index] = false;
                    break;
                }
            }
        }
        
        // 只是為了把非 subset 的存進一個變數，好回傳
        for (int i = 0; i < favoriteCompanies.size(); ++i) {
            if (isNotSubset[i]) {
                theList.push_back(i);
            }
        }
        
        return theList;
        
    }
    
    static bool cmp1(vector<string>& a, vector<string>& b) {
        return a.size() < b.size();
    }
    
    static bool cmp2(string& a, string& b) {
        return a < b;
    }
};
```

### 其他人的做法

[C++ includes](https://leetcode.com/problems/people-whose-list-of-favorite-companies-is-not-a-subset-of-another-list/discuss/636363/C%2B%2B-includes)

- 用 `unordered_map<string, int>` 將公司名稱 (string) 轉換為獨一無二的 id (int)，這樣後需的操作會比較容易
- 用 `includes()` 判斷兩 sorted range 是否有 include 的關係

```cpp
vector<int> peopleIndexes(vector<vector<string>>& favs) {
    unordered_map<string, int> ids;
    int id = 0;
    vector<vector<int>> comps(favs.size());
    for (auto i = 0; i < favs.size(); ++i) {
        for (auto &comp : favs[i]) {
            if (!ids.count(comp)) {
                ids[comp] = id++;
            }
            comps[i].push_back(ids[comp]);
        }            
    }
    for (auto &comp: comps)
        sort(begin(comp), end(comp));
    vector<int> res;
    for (auto i = 0; i < comps.size(); ++i) {
        bool notSubset = true;            
        for (auto j = 0; j < comps.size() && notSubset; ++j) {
            if (i == j)
                continue;
            notSubset = !includes(begin(comps[j]), end(comps[j]), begin(comps[i]), end(comps[i]));
        }
        if (notSubset)
            res.push_back(i);
    }
    return res;
}
```

### 可能對未來解題有幫助

1. `int` to `string`

    ```cpp
    string s = to_string(30);
    ```

2. `string` to `int`

    ```cpp
    int stoi(string)

    long stol(string)
    float stof(string)
    double stod(string)
    ```
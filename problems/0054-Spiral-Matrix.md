## 54. Spiral Matrix (Medium)

[題目連結](https://leetcode.com/problems/spiral-matrix/)

### 我的解題思路

在紙上模擬找規律，發現水平方向、垂直方向的移動次數逐漸遞減，以及輪流用四種 mode 去移動
（code 亂到可能自己以後看會不知道自己在寫什麼）

### C++

```cpp
// 2021-09-17 星期五
// Runtime: 0 ms (100.00%)
// Memory Usage: 6.9 MB (72.25%)

class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        
        vector<int> result;
        
        int row = matrix.size();
        int col = matrix[0].size();
        int h = col;
        int v = row;
        
        int x = -1;
        int y = 0;
        int mode = 0;
        int tempH = h;
        int tempV = v;
        
        while (mode % 2 == 0 && h != 0 || mode % 2 == 1 && v != 0) {
            
            switch(mode) {
                case 0: tempH--, ++x; break;
                case 1: tempV--, ++y; break;
                case 2: tempH--, --x; break;
                case 3: tempV--, --y; break;
            }
            
            result.push_back(matrix[y][x]);
            
            if (tempH == 0 || tempV == 0) {
                mode = (mode + 1) % 4;
                switch(mode) {
                    case 0:
                    case 2: h--; break;
                    case 1: 
                    case 3: v--; break;
                }
                tempH = h;
                tempV = v;
            }
        }
        
        return result;
        
    }
};
```
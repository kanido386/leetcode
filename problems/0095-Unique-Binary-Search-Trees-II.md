## 95. Unique Binary Search Trees II (Medium)

[題目連結](https://leetcode.com/problems/unique-binary-search-trees-ii/)

本題耗時：不計（有參考別人的code）

### 我的解題思路

在紙上模擬的時候有回憶起什麼 BST，但實作上就卡住了⋯⋯
後來參考其他人寫的 code，才知道原來能用 Divide and Conquer 的概念去解！

### C++

```cpp
// 2021-09-03 星期五
// Runtime: 26 ms (12.53%)
// Memory Usage: 16.2 MB (36.77%)

/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<TreeNode*> generateTrees(int n) {
        return helper(1, n);
    }
    
    vector<TreeNode*> helper(int start, int end) {
        
        // 可能是因為沒這個，就不會有 null 吧(?)
        if (end < start)
            return vector<TreeNode*>{nullptr};
        
        vector<TreeNode*> ans;
        for (int i = start; i <= end; ++i) {
            vector<TreeNode*> leftSubtree = helper(start, i-1);
            vector<TreeNode*> rightSubtree = helper(i+1, end);
            for (auto left : leftSubtree) {
                for (auto right : rightSubtree) {
                    TreeNode* root = new TreeNode(i);
                    root->left = left;
                    root->right = right;
                    
                    // 一直很疑惑這點，但後來想通了：
                    // return 出來的 "ans" vector 會給上層取用，不全是要當作 output
                    ans.push_back(root);
                }
            }
        }
        return ans;
    }
};
```

### 可能對未來解題有幫助

- [struct initialization in c++](https://stackoverflow.com/questions/5800585/regarding-struct-initialization-in-c)

    ```cpp
    struct ABC
    {
       int x;
       int y;

       ABC(): x(1),y(2){}
    };

    ABC s; // x and y initialized to 1 and 2 respectively
    ABC* test = new ABC;
    ```

- 都快忘記怎麼 [get the value of a pointer](https://stackoverflow.com/questions/14420257/cpp-c-get-pointer-value-or-depointerize-pointer)

    ```cpp
    int *ptr;
    int value;
    *ptr = 9;

    value = *ptr;
    ```

- 居然也快忘記怎麼 [passing pointer to function](https://stackoverflow.com/questions/3796181/c-passing-pointer-to-function-howto-c-pointer-manipulation)

    ```cpp
    void Foo(int *x);

    int x = 4;
    int *x_ptr = &x;
    Foo(x_ptr);
    Foo(&x);
    ```

- arrow operator

    ```cpp
    sample *sam = new sample();
    (*sam).init(100);
    sam->init(100);
    ```
# Tree
### Structure:
```cpp
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
```
### Preorder Traversal (LeetCode 144)
#### Recursion
```cpp
class Solution {
public:
    vector<int> result;
    vector<int> preorderTraversal(TreeNode* root) {
        traverse(root);
        return result;
    }
    void traverse(TreeNode* root) {
        if (root == NULL) return;
        result.push_back(root->val);
        traverse(root->left);
        traverse(root->right);
    }
};
```
#### Iteration
```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();
            result.push_back(cur->val);
            if (cur->right) st.push(cur->right);
            if (cur->left) st.push(cur->left);
        }
        return result;
    }
};
```
### Inorder Traversal (LeetCode 95)
#### Recursion
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        traverse(root, res);
        return res;
    }
    
    void traverse(TreeNode* root, vector<int>& res) {
        if (root == NULL) return;
        traverse(root->left, res);
        res.push_back(root->val);
        traverse(root->right, res);
    }
};
```
#### Iteration
```cpp
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        TreeNode* cur = root;
        while (cur || !st.empty()) {
            if (cur) {
                st.push(cur);
                cur = cur->left;
            }
            else {
                cur = st.top();
                st.pop();
                result.push_back(cur->val);
                cur = cur->right;
            }
        }
        return result;
    }
};
```
### Postorder Traversal (LeetCode 145)
#### Recursion
```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> result;
        traversal(root, result);
        return result;
    }
    void traversal(TreeNode* root, vector<int>& result) {
        if (root == NULL) return;
        traversal(root->left, result);
        traversal(root->right, result);
        result.push_back(root->val);
    }
};
```
#### Iteration
```cpp
class Solution {
public:
    vector<int> postorderTraversal(TreeNode* root) {
        stack<TreeNode*> st;
        vector<int> result;
        if (root == NULL) return result;
        st.push(root);
        while (!st.empty()) {
            TreeNode* cur = st.top();
            st.pop();
            result.push_back(cur->val);
            if (cur->left) st.push(cur->left);
            if (cur->right) st.push(cur->right);
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```
### Level Order Traversal (LeetCode 122)
```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> que;
        vector<vector<int>> result;
        if (root == NULL) return result;
        que.push(root);
        while (!que.empty()) {
            int size = que.size();
            vector<int> vec;
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                vec.push_back(cur->val);
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
            result.push_back(vec);
        }
        return result;
    }
};
```
### Application 1: Binary Tre Iterator (LeetCode 173)
Solution 1: Traversal the whole tree and then build the iterator
```cpp
class BSTIterator {
private:
    vector<int> vec;
    int idx;
    void traversal(TreeNode* root, vector<int>& res) {
        if (root == NULL) return;
        traversal(root->left, res);
        res.push_back(root->val);
        traversal(root->right, res);
    }
    vector<int> inorder(TreeNode* root) {
        vector<int> res;
        traversal(root, res);
        return res;
    }
public:
    BSTIterator(TreeNode* root): idx(0), vec(inorder(root)) {
        
    }
    
    int next() {
        int cur = vec[idx];
        idx++;
        return cur;
    }
    
    bool hasNext() {
        return idx < vec.size();
    }
};
```
Solution 2: Iterate the tree while iterator is working
```cpp
class BSTIterator {
private:
    TreeNode* cur;
    stack<TreeNode*> stk;
public:
    BSTIterator(TreeNode* root): cur(root) {}
    
    int next() {
        while (cur != NULL) {
            stk.push(cur);
            cur = cur->left;
        }
        cur = stk.top();
        stk.pop();
        int ret = cur->val;
        cur = cur->right;
        return ret;
    }
    
    bool hasNext() {
        return cur != NULL || !stk.empty();
    }
};

/**
 * Your BSTIterator object will be instantiated and called as such:
 * BSTIterator* obj = new BSTIterator(root);
 * int param_1 = obj->next();
 * bool param_2 = obj->hasNext();
 */
```

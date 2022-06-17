# Pointers vs References in C++
Link: https://www.geeksforgeeks.org/pointers-vs-references-cpp/

## Initialization:
### Pointer Initialization:
```
int a = 10;
int *p = &a; //This means "I am declaring a pointer p of a", where p is the address of a and *p is the content of a
OR
int *p; //"I am declaring a pointer named p"
p = &a; //"pointer p contains the address of a"
```
### Reference Initialization:
```
int a = 10;
int &p = a;
```


## Example: LeetCode 230: Kth Smallest Element in a BST
The idea is to use a inorder traversal to traverse the BST and find the kth traversal result. We can declare the index and result variable as global variable like this:
### Global Variable:
```
class Solution {
public:
    int i = 0;
    int res = 0;
    int kthSmallest(TreeNode* root, int k) {
        traverse(root, k);
        return res;
    }
    void traverse(TreeNode* root, int k) {
        if (root == NULL) return;
        traverse(root->left, k);
        i++;
        if (i == k) {
            res = root->val;
            return;
        }
        traverse(root->right, k);
        return;
    }
};
```
However, if we donot want to use global variable, we can define i and res in kthSmallest function and use them by reference or pointer in traverse function.
### Use as reference:
```
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        int i = 0;
        int res = 0;
        traverse(root, k, i, res);
        return res;
    }
    //In this case, "int& _i, int& _res" are references of "i, res" in kthSmallest function. 
    //it means &_i = i and &_res = res, where _i and _res are references of i and res
    void traverse(TreeNode* root, int k, int& _i, int& _res) {
        if (root == NULL) return;
        traverse(root->left, k, _i, _res);
        _i++;
        if (_i == k) {
            _res = root->val;
            //cout<<root->val<<endl;
            return;
        }
        traverse(root->right, k, _i, _res);
        return;
    }
};
```
### Use as pointer:
```
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        int i = 0;
        int res = 0;
        traverse(root, k, &i, &res);
        return res;
    }
    //We want traverse function to get and change the content of "i" and "res", 
    //so we need to make a pointer like *_i = &i, *_res = &res
    void traverse(TreeNode* root, int k, int* _i, int* _res) {
        //int*_i and int*_res are addresses because they are from (*_i = &i), 
        //but in beow when we use *_i (for example, when we use (*_i)++ below), _i means address and *_i means the content
        if (root == NULL) return;
        traverse(root->left, k, _i, _res);
        (*_i)++;
        //_i is an address and *_i is the value of the address
        if ((*_i) == k) {
            (*_res) = root->val;
            //cout<<root->val<<endl;
            return;
        }
        traverse(root->right, k, _i, _res);
        return;
    }
};
```

## Tree
a->b equivalent to (*a).b
root->val equivalent to (*root).val, where root is an address, *root is the content

# LeetCodeNote

## Binary Search
### 704
Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1. You must write an algorithm with O(log n) runtime complexity. You must write an algorithm with O(log n) runtime complexity.
> Example 1:  
Input: nums = [-1,0,3,5,9,12], target = 9  
Output: 4  
Explanation: 9 exists in nums and its index is 4  

> Example 2:  
Input: nums = [-1,0,3,5,9,12], target = 2  
Output: -1  
Explanation: 2 does not exist in nums so return -1  

Solution1: Left close, right close:
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;
            if (nums[mid] > target) right = mid - 1;
            if (nums[mid] < target) left = mid + 1;
        }
        return -1;
    }
};
//nums = [-1, 0, 3, 5, 9, 12], target = 8
//left = 0, right = 5, left < right, mid = 2, nums[mid] = 3 < target, left = 3
//left = 3, right = 5, left < right, mid = 4, nums[mid] = 9 > target, right = 3
//left = 3, right = 3, left = right, mid = 3, nums[mid] = 5 < target, left = 4
//left > right, break, return -1;
```
Solution2: Left close, right open:
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size();
        while (left < right) { //Difference1
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;
            if (nums[mid] > target) right = mid; //Difference2
            if (nums[mid] < target) left = mid + 1;
        }
        return -1;
    }
};
//nums = [-1, 0, 3, 5, 9, 12], target = 8
//left = 0, right = 6, left < right, mid = 3, nums[mid] = 5 < target, left = 4
//left = 4, right = 6, left < right, mid = 5, nums[mid] = 12 > target, right = 5
//left = 4, right = 5, left < right, mid = 4, nums[mid] = 9 > target, right = 4
//left = right, break, return -1;
```
### 33 Search in Rotated Sorted Array
There is an integer array nums sorted in ascending order (with distinct values).
Prior to being passed to your function, nums is possibly rotated at an unknown pivot index k (1 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,5,6,7] might be rotated at pivot index 3 and become [4,5,6,7,0,1,2].
Given the array nums after the possible rotation and an integer target, return the index of target if it is in nums, or -1 if it is not in nums.
You must write an algorithm with O(log n) runtime complexity.
>Example 1:  
Input: nums = [4,5,6,7,0,1,2], target = 0  
Output: 4  

>Example 2:  
Input: nums = [4,5,6,7,0,1,2], target = 3  
Output: -1  

>Example 3:  
Input: nums = [1], target = 0  
Output: -1  
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return mid;
            if (nums[mid] > target) {
                if (nums[mid] >= nums[left] && nums[left] > target) left = mid + 1; // mid = left is possible
                else right = mid - 1;
            }
            if (nums[mid] < target) {
                if (target > nums[right] && nums[right] >= nums[mid]) right = mid - 1; // mid = right is possible
                else left = mid + 1;
            }
        }
        return -1;
    }
};
//nums[mid] > target: separate nums into two ascending parts, the first part is always "higher" than the second part. 
//If nums[mid] and target are in the same part, we could use the normal binary search logic. 
//However, if nums[mid] and target are in the different part, (if nums[mid] > target, 
//then nums[mid] will be in the first part and target will be in the second part), 
//then there must be nums[mid] >= nums[left] > target (nums[mid] could equal to target 
//but nums[left] will not be equal to target), and we will make left = mid + 1;

//nums[mid] < target:
//Same to the previous scenario, if nums[mid] in second part and target in first part, 
//then there will be target > nums[right] >= nums[mid] and we will make right = mid - 1;
```
### 81 Search in Rotated Sorted Array II
There is an integer array nums sorted in non-decreasing order (not necessarily with distinct values).
Before being passed to your function, nums is rotated at an unknown pivot index k (0 <= k < nums.length) such that the resulting array is [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]] (0-indexed). For example, [0,1,2,4,4,4,5,6,6,7] might be rotated at pivot index 5 and become [4,5,6,6,7,0,1,2,4,4].
Given the array nums after the rotation and an integer target, return true if target is in nums, or false if it is not in nums.
You must decrease the overall operation steps as much as possible.
> Example 1:  
Input: nums = [2,5,6,0,0,1,2], target = 0  
Output: true  

> Example 2:  
Input: nums = [2,5,6,0,0,1,2], target = 3  
Output: false  
```
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] == target) return true;
            if (nums[mid] == nums[left] && nums[mid] == nums[right]) {
                left++;
                right--;
            } // added compare to problem 33
            else if (nums[mid] > target) {
                if (nums[mid] >= nums[left] && nums[left] > target) left = mid + 1;
                else right = mid - 1;
            }
            else if (nums[mid] < target) {
                if (target > nums[right] && nums[right] >= nums[mid]) right = mid - 1;
                else left = mid + 1;
            }
        }
        return false;
    }
};
//Compare to 33, there are more special cases. For example, [1, 1, 0, 1], [1, 1, 2, 1], where nums[left] == nums[right] == nums[mid], 
//in this case, there is no way to only move left or right, the only way is to shrink both left and right. 
//The worst case is that every element in the array is the same, which will take O(n) time complexity. 
//For an array with less duplicated element, O could reach O(logn)
```
## Pointers
### 912 Sort an Array
Given an array of integers nums, sort the array in ascending order.
> Example 1:  
Input: nums = [5,2,3,1]  
Output: [1,2,3,5]  

>Example 2:  
Input: nums = [5,1,1,2,0,0]  
Output: [0,0,1,1,2,5]  

Method 1: Merge Sort, Sort left, Sort right, Merger left and right with two pointers
```
```
Method 2: Quick Sort, 
```
```
### 75 Sort Colors
Given an array nums with n objects colored red, white, or blue, sort them in-place so that objects of the same color are adjacent, with the colors in the order red, white, and blue.
We will use the integers 0, 1, and 2 to represent the color red, white, and blue, respectively.
You must solve this problem without using the library's sort function.
> Example 1:  
Input: nums = [2,0,2,1,1,0]  
Output: [0,0,1,1,2,2]  

> Example 2:  
Input: nums = [2,0,1]  
Output: [0,1,2]  

Solution 1: Two pointers for 0 and 1
```
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int p0 = 0, p1 = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == 0) {
                swap(nums[i], nums[p0]);
                if (p0 < p1) swap(nums[i], nums[p1]);
                p0++;
                p1++;
            }
            else if (nums[i] == 1) {
                swap(nums[i], nums[p1]);
                p1++;
            }
        }
    }
};
//every time after updating p0 and p1, p0 and p1 will be always in the next position of the sorted "0"s and "1"s. 
//For example, [2,0,2,1,1,0], after several iteration becames [0,1,1,2,2,0], where i = 5, p0 = 1 and p1 = 3. 
//Now nums[i] == 0, we swap nums[i] and nums[p0] first, but we swap the sorted "1" in p0 position to nums[i] position, 
//so we need to swap(nums[i], nums[p1]) to get that "1" back to the last element of the sorted "1"s.
```
Solution 2: Two pointers for 0 and 2
```
class Solution {
public:
    void sortColors(vector<int>& nums) {
        int p0 = 0, p2 = nums.size() - 1;
        for (int i = 0; i <= p2; i++) {
            while (nums[i] == 2 && i <= p2) {
                swap(nums[i], nums[p2]);
                p2--;
            }
            if (nums[i] == 0) {
                swap(nums[i], nums[p0]);
                p0++;
            }
        }
    }
};
//Out i is going from left to right, so each time when nums[i]==2, we swap(nums[i], nums[p2]) 
and make p2-- to make sure p2 is always the previous index of the sorted "2"s. 
However, if current nums[p2]=2, the swap does not make any difference, 
so we have to use a while loop to change the first "non-2" element met by p2 and make nums[i] not 2. 
In this case, nums[i] could be 0 or 1, and we check nums[i] is 0 or not, 
that's why we need to put "if (nums[i] == 0)" after the condition of p2.
```
### 21 Merge Two Sorted Lists
You are given the heads of two sorted linked lists list1 and list2.
Merge the two lists in a one sorted list. The list should be made by splicing together the nodes of the first two lists.
Return the head of the merged linked list.
> Example 1:  
Input: list1 = [1,2,4], list2 = [1,3,4]  
Output: [1,1,2,3,4,4]  

> Example 2:  
Input: list1 = [], list2 = []  
Output: []  

> Example 3:  
Input: list1 = [], list2 = [0]  
Output: [0]  

Solution:
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* dummy = new ListNode(0);
        ListNode* p = dummy, *p1 = list1, *p2 = list2;
        while (p1 && p2) {
            if (p1->val < p2->val) {
                p->next = p1;
                p1 = p1->next;
            }
            else {
                p->next = p2;
                p2 = p2->next;
            }
            p = p->next;
        }
        if (p1) p->next = p1;
        if (p2) p->next = p2;
        return dummy->next;
    }
};
//1. Remember the defination of listnode
//2. Dont forget the p = p->next and if p1 or p2 exist after while
```
### 3 Longest Substring Without Repeating Characters
Given a string s, find the length of the longest substring without repeating characters.
> Example 1:  
Input: s = "abcabcbb"  
Output: 3  
Explanation: The answer is "abc", with the length of 3.  

> Example 2:  
Input: s = "bbbbb"  
Output: 1  
Explanation: The answer is "b", with the length of 1.  

> Example 3:  
Input: s = "pwwkew"  
Output: 3  
Explanation: The answer is "wke", with the length of 3.  
Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.  

Solution: Use a slicing window
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int left = 0, res = 0;
        unordered_set<char> window;
        for (int i = 0; i < s.size(); i++) {
            //window starts at left and end at i;
            while (window.find(s[i]) != window.end()) {
                window.erase(s[left]);
                left++;
            }
            window.insert(s[i]);
            res = max(res, i - left + 1);
        }
        return res;
    }
};
```
### 53 Maximum Subarray
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
A subarray is a contiguous part of an array.
> Example 1:  
Input: nums = [-2,1,-3,4,-1,2,1,-5,4]  
Output: 6  
Explanation: [4,-1,2,1] has the largest sum = 6.  

> Example 2:  
Input: nums = [1]  
Output: 1  

> Example 3:  
Input: nums = [5,4,-1,7,8]  
Output: 23  

Solution:
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        // dp
        vector<int> dp(nums.size());
        //Define dp[i] as the max sum subarray end with i
        dp[0] = nums[0];
        int res = dp[0];
        for (int i = 1; i < nums.size(); i++) {
            dp[i] = max(nums[i], dp[i - 1] + nums[i]);
            res = max(res, dp[i]);
        }
        return res;
        
        // Brute force (Time limit Exceeded)
        // int res = nums[0];
        // for (int i = 0; i < nums.size(); i++) {
        //     int cur = 0;
        //     for (int j = i; j >= 0; j--){
        //         cur += nums[j];
        //         res = max(cur, res);
        //     }
        // }
        // return res;
    }
};
```
### 1 Two Sum
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.
You may assume that each input would have exactly one solution, and you may not use the same element twice.
You can return the answer in any order.
> Example 1:  
Input: nums = [2,7,11,15], target = 9  
Output: [0,1]  
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].  

> Example 2:  
Input: nums = [3,2,4], target = 6  
Output: [1,2]  

> Example 3:  
Input: nums = [3,3], target = 6  
Output: [0,1]  

Solution:
```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); i++) {
            int goal = target - nums[i];
            // if (map.find(goal) != map.end()) return {i, map[goal]};
            // map.insert({nums[i], i});
            //Another way to iterate:
            auto it = map.find(goal);
            if (it != map.end()) return {it->second, i};
            map[nums[i]] = i;
        }
        return {};
    }
};
```
## BFS
### 297 Serialize and Deserialize Binary Tree
Solution1: DFS (recursion in pre-order)
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    void traverse(TreeNode* root, string& str) {
        if (root == NULL) str += "#,";
        else {
            str += to_string(root->val) + ",";
            traverse(root->left, str);
            traverse(root->right, str);
        }
    }
    
    string serialize(TreeNode* root) {
        string str;
        traverse(root, str);
        return str;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        queue<string> que; // que can better pop put front
        string temp;
        for (char c: data) {
            if (c == ',') {
                que.emplace(temp);
                temp.clear();
            }
            else temp.push_back(c);
        }
        return build(que);
    }
    
    TreeNode* build(queue<string>& que) {
        if (que.front() == "#") {
            que.pop();
            return NULL;
        }
        TreeNode* root = new TreeNode(stoi(que.front()));
        que.pop();
        root->left = build(que);
        root->right = build(que);
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```
Solution2: BFS
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    void traverse(TreeNode* root, string& str) {
        queue<TreeNode*> que;
        que.push(root);
        //str += to_string(root->val) + ",";
        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                if (cur == NULL) str += "#,";
                else {
                    str += to_string(cur->val) + ",";
                    que.push(cur->left);
                    que.push(cur->right);
                }
            }
        }
    }
    
    string serialize(TreeNode* root) {
        string str;
        traverse(root, str);
        return str;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        queue<string> str; // queue can better pop put front
        string temp;
        for (char c: data) {
            if (c == ',') {
                str.emplace(temp);
                temp.clear();
            }
            else temp.push_back(c);
        }
        return build(str);
    }
    
    TreeNode* build(queue<string>& str) {
        if (str.front() == "#") return NULL;
        TreeNode* root = new TreeNode(stoi(str.front()));
        queue<TreeNode*> que;
        que.push(root);
        str.pop();
        while (!que.empty() && !str.empty()) {
            TreeNode* cur = que.front();
            que.pop();
            if (str.front() != "#") {
                TreeNode* left = new TreeNode(stoi(str.front()));
                cur->left = left;
                que.push(left);
                str.pop();
            }
            else {
                cur->left = NULL;
                str.pop();
            }
            if (str.front() != "#") {
                TreeNode* right = new TreeNode(stoi(str.front()));
                cur->right = right;
                que.push(right);
                str.pop();
            }
            else {
                cur->right = NULL;
                str.pop();
            }
        }
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```
### 210

### 200

### 133

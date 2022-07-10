# Binary Search
## Binary Search Notes
### Find boundary
#### Left close, right open:
```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        return {getLeft(nums, target), getRight(nums, target)};
    }
    int getLeft(vector<int>& nums, int target) {
        if (nums.size() == 0) return -1;
        int left = 0, right = nums.size();
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] >= target) right = mid;
            else if (nums[mid] < target) left = mid + 1;
        }
        if (left < nums.size() && nums[left] == target) return left;
        else return -1;
    }
    int getRight(vector<int>& nums, int target) {
        if (nums.size() == 0) return -1;
        int left = 0, right = nums.size();
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) right = mid;
            else if (nums[mid] <= target) left = mid + 1;
        }
        if (right - 1 >= 0 && nums[right - 1] == target) return right - 1;
        else return -1;
    }
};
```
#### Left close, right close
```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int n = nums.size();
        if (n == 0) return {-1, -1};
        return {leftBound(nums, target), rightBound(nums, target)};
    }
    int leftBound(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                right = mid - 1;
            }
            else if (nums[mid] < target) {
                left = mid + 1;
            }
            else if (nums[mid] == target) {
                right = mid - 1;
            }
        }
        if ( left > nums.size() - 1 || nums[left] != target) return -1;
        else return left;
    }
    int rightBound(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) {
                right = mid - 1;
            }
            else if (nums[mid] < target) {
                left = mid + 1;
            }
            else if (nums[mid] == target) {
                left = mid + 1;
            }
        }
        if (right < 0 || nums[right] != target) return -1;
        else return right;
    }
};
```

Example1
```cpp
class Solution {
public:
    vector<int> presum;
    Solution(vector<int>& w) {
        presum.resize(w.size());
        presum[0] = w[0];
        for (int i = 1; i < w.size(); i++) {
            presum[i] = presum[i - 1] + w[i];
        }
    }
    
    int pickIndex() {
        int randn = rand() % presum.back();
        int left = 0, right = presum.size();
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (presum[mid] > randn) right = mid;
            else if (presum[mid] <= randn) left = mid + 1;
        }
        return right;
        //First, we need to make sure which bound we want to search. For example, w = [1, 3], presum = [1, 4], if weight = 1, we want to return index 1, hence weight = 1 should be in the "1-4" range of presum, thus we want to search the right bound of weight should be inserted in presum. for left close right open search, the right result will just be right
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(w);
 * int param_1 = obj->pickIndex();
 */
```
#### Left close right close
```cpp
class Solution {
public:
    vector<int> presum;
    Solution(vector<int>& w) {
        presum.resize(w.size());
        presum[0] = w[0];
        for (int i = 1; i < w.size(); i++) {
            presum[i] = presum[i - 1] + w[i];
        }
    }
    
    int pickIndex() {
        int randn = rand() % presum.back();
        int left = 0, right = presum.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (presum[mid] > randn) right = mid - 1;
            else if (presum[mid] <= randn) left = mid + 1;
        }
        return right + 1;
        //If its left close right close, we still need to search right bound because the rand and presum are not changed. for left close right close, left bound is left and right bound is right, but we want the "right inserted bound", so need to return right + 1;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(w);
 * int param_1 = obj->pickIndex();
 */
```

### Summary
Left Close Right Close
```cpp
int left = 0, right = nums.size() - 1;
while (left <= right) {
    if (nums[mid] == target) // left bound: right = mid - 1; rightbound: left = mid + 1;
}
return left (if find left bound), right (if find right bound)
// left bound = left inserted bound, right bound = right insertion bound  - 1;
for example, [1,2,3,3,5], find 3, lb = lib = 2, rb = 3, rib = 4 (when while loop end, left = 2 and right = 3)
Another example, [1,2,3,3,5], find 4, lb = lib = 4, rb = 3, rib = 4 (when while loop end, left = 4 and right = 3)
```
Left CLose Right Open
```cpp
int left = 0, right = nums.size();
while (left < right) {
    if (nums[mid] == target) // left bound: right = mid; right bound: left = mid + 1;
}
return left if finding leftbound or right - 1 if find rightbound
left will be the left inserted position, right will be the right inserted position, so right - 1 will be the right bound
while loop will end when left == right, so return left or right does not matter
```

## Binary Search Examples
### LeetCode704
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
```cpp
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
```cpp
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
### LeetCode33 Search in Rotated Sorted Array
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
```cpp
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
### LeetCode81 Search in Rotated Sorted Array II
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
```cpp
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

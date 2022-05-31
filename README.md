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

## 33

## 81

# BackTracking:
All we need is to understand this article:
https://labuladong.gitee.io/algo/4/29/105/
|                      | Unique Element && Unique Choose | Unique Element && Repeat Choose | Repeat Element && Unique Choose |
| -------------------- | ------------------------------- | ------------------------------- | ------------------------------- |
| Permutation          | Scenario 1 (LeetCode 77)        |            Senario 3            |            Senario 5            |
| Combination / Subset | Scenario 2 (LeetCode 46, 78)    |            Senario 4            |            Senario 6            |

## Scenario 1: Unique Element && Unique Choose, Permutation
```cpp
vector<bool> used(nums.size(), false);
vector<int> path;
void backtracking(vector<int> nums) {
    if (ending condition)
    for (int i = 0; ..) {
        if (used[i]) continue;
        used[i] = true;
        path.push_back(nums[i]);
        backtracking(nums)
        used[i] = false;
        path.pop_back();
    }
}
```
## Scenario 2: Unique Element && Unique Choose, Combination / Subset
```cpp
void backtracking(vector<int> nums, int start) {
    if (ending condition)
    for (int i = start; ..) {
        path.push_back(nums[i]);
        backtracking(nums, i + 1);
        path.pop_back();
    }
}
```
## Scenario 3: Unique Element && Repeat Choose, Permutation
```cpp
void backtracking(vector<int> nums) {
    if (ending condition);
    for (int i = 0; ..) {
        path.push_back(nums[i]);
        backtracking(nums);
        path.pop_back();
    }
}
```
## Scenario 4: Unique Element && Repeat Choose, Combination / Subset
```cpp
void backtracking(vector<int> nums, int start) {
    if (ending condition);
    for (int i = start; ..) {
        path.push_back(nums[i]);
        backtracking(nums, i); //Compare to Scenario2, this is the only difference
        path.pop_back();
    }
}
```
## Scenario 5: Repeat Element && Unique Choose, Permutation
```cpp
vector<bool> used(nums.size(), false);
vector<vector<int>> permutation(vector<int> nums) {
    sort(nums.begin(), nums.end());
}
void backtracking(vector<int> nums) {
    if (ending condition);
    for (int i = 0; ..) {
        if (used[i]) continue;
        if (i > 0 && nums[i] == nums[i - 1] && !used[i - 1]) continue;
        used[i] = true;
        path.push_back(nums[i]);
        backtracking(nums);
        used[i] = false;
        path.pop_back();
    }
}
```
## Scenario 6: Repeat Element && Unique Choice, Combination / Subset
```cpp
vector<vector<int>> combination(vector<int> nums) {
    sort(nums.begin(), nums.end());
}
void backtracking(vector<int> nums, int start) {
    if (ending condition);
    for (int i = start; ..) {
        if (i > start && nums[i] == nums[i - 1]) continue;
        path.push_back(nums[i]);
        backtracking(nums, i + 1);
        path.pop_back();
    }
}
```
### 77 Permutation, Unique Element, Unique Choose (Scenario 1 example)
```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    vector<vector<int>> combine(int n, int k) {
        backtracking(n, 1, k);
        return result;
    }
    void backtracking(int n, int idx, int k) { //Backtracking defination is a little different than 78_subset
        if (path.size() == k) {
            result.push_back(path);
            return;
        }
        for (int i = idx; i <= n; i++) {
            path.push_back(i);
            backtracking(n, i + 1, k);
            path.pop_back();
        }
    }
};
```
### 78 Subset, Unique Element, Unique Choose (Scenario 2 example)
```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    vector<vector<int>> subsets(vector<int>& nums) {
        backtracking(nums, 0);
        return result;
    }
    void backtracking(vector<int>& nums, int idx) {
        result.push_back(path);
        for (int i = idx; i < nums.size(); i++) {
            path.push_back(nums[i]);
            backtracking(nums, i + 1);
            path.pop_back();
        }
    }
};
```
### 46 Combination, Unique Element, Unique Choose (Scenario 2 example)
```cpp
class Solution {
public:
    vector<vector<int>> result;
    vector<int> path;
    vector<bool> used;
    vector<vector<int>> permute(vector<int>& nums) {
        used.resize(nums.size());
        backtracking(nums);
        return result;
    }
    void backtracking(vector<int>& nums) {
        if (path.size() == nums.size()) result.push_back(path); // Donot need to return, if path size = nums size then it won't go into for loop
        for (int i = 0; i < nums.size(); i++) {
            if (used[i]) continue; //If used in current path, cannot use it again for example [1, 2, 3] might have [1, 2, 1] as result if not use used vector
            path.push_back(nums[i]);
            used[i] = true;
            backtracking(nums);
            path.pop_back();
            used[i] = false;
        }
    }
};
```

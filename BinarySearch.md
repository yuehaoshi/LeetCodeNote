Find boundary
Left close, right open:
```
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
Left close, right close
```
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
```
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
Left close right close
```
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

Summary
Left Close Right Close
```
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
```
int left = 0, right = nums.size();
while (left < right) {
    if (nums[mid] == target) // left bound: right = mid; right bound: left = mid + 1;
}
return left if finding leftbound or right - 1 if find rightbound
left will be the left inserted position, right will be the right inserted position, so right - 1 will be the right bound
while loop will end when left == right, so return left or right does not matter
```

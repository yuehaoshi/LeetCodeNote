C++ STD Sort can be passed by a compare function to compare the desired element for each sorted element. 
For example, if we want to sort a 2d array by the second element in interval vector, then we can do 
```
private:
static bool cmp(const vector<int>& a, vector<int>& b) {
    return a[1] < b[1];
}
public:
void sortVec(vector<vector<int>> &vec) {
    sort(vec.begin(), vec.end(), cmp);
}
```

For example, leetcode 435 non-overlapping intervals
```
class Solution {
private:
    static bool cmp(const vector<int>& a, const vector<int>& b) {
        return a[1] < b[1];
    }
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if (intervals.size() == 0) return 0;
        sort(intervals.begin(), intervals.end(), cmp);
        int cur_end = intervals[0][1];
        int count = 1;
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals[i][0] >= cur_end) {
                count++;
                cur_end = intervals[i][1];
            }
        }
        return intervals.size() - count;
    }
};
```

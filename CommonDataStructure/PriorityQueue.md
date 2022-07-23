`priority_queue<int> pq`, if we push element into this pq, the largest element will always be on the top()
If we want to make a "small element to the top" pq, we can do this:
`priority_queue<int, vector<int>, greater<int>> pq`

To make a customiazed comparator, do this:
leetcode 23
```cpp
struct cmp {
    bool operator() (ListNode* a, ListNode* b) {
        return a->val > b->val;
    }
};

class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        if (lists.size() == 0) return NULL;
        priority_queue<ListNode*, vector<ListNode*>, cmp> que;
        for (auto head: lists) {
            if (head != NULL) que.push(head);
        }
        ListNode* dummy = new ListNode();
        ListNode* p = dummy;
        while (!que.empty()) {
            ListNode* cur = que.top();
            if (cur->next) que.push(cur->next);
            que.pop();
            p->next = cur;
            p = p->next;
        }
        return dummy->next;
    }
};
```
Reference: 
http://neutrofoton.github.io/blog/2016/12/29/c-plus-plus-priority-queue-with-comparator/
https://jimmy-shen.medium.com/customize-comparison-in-c-fa48c0eac6d8

pq custom comparator (leetcode 1851)
```
class Compare {
public: 
    bool operator() (vector<int>& vec1, vector<int>& vec2) {
        int len1 = vec1[1] - vec1[0];
        int len2 = vec2[1] - vec2[0];
        return len1 > len2;
    }
};

class Solution {
public:
    vector<int> minInterval(vector<vector<int>>& intervals, vector<int>& queries) {
        vector<int> res(queries.size(), -1);
        vector<pair<int, int>> newQueries;
        for (int i = 0; i < queries.size(); i++) newQueries.push_back({queries[i], i});
        sort(newQueries.begin(), newQueries.end());
        sort(intervals.begin(), intervals.end());
        priority_queue<vector<int>, vector<vector<int>>, Compare> que;
        int idx = 0;
        for (int i = 0; i < queries.size(); i++) {
            while (idx < intervals.size() && intervals[idx][0] <= newQueries[i].first) {
                que.push(intervals[idx]);
                idx++;
            }
            while (!que.empty() && que.top()[1] < newQueries[i].first) {
                que.pop();
            }
            if (!que.empty()) {
                res[newQueries[i].second] = que.top()[1] - que.top()[0] + 1;
            }
        }
        return res;
    }
};
```

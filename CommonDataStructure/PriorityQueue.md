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

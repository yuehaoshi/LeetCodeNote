# Heap
#### 23 Merge k Sorted Lists
```cpp
class Solution {
public:
    struct comp {
        bool operator()(ListNode* a, ListNode* b) {
            return a->val > b->val;
        }
    };
    priority_queue<ListNode*, vector<ListNode*>, comp> q;
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        for (auto node:lists) {
            if (node) q.push(node);
        }
        ListNode* head = new ListNode();
        ListNode* tail = head;
        while (!q.empty()) {
            ListNode* node = q.top();
            q.pop();
            tail->next = node;
            tail = tail->next;
            if (node->next) q.push(node->next);
        }
        return head->next;
    }
};
```

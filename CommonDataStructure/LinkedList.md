# Singly-Linked List
Definition
```cpp
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
```
#### 2 Add Two Numbers
```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *p1 = l1, *p2 = l2, *dummy = new ListNode(-1);
        ListNode *p = dummy;
        int carry = 0;
        while (p1 || p2 || carry) {
            int val = 0;
            if (carry) val += carry;
            if (p1) {
                val += p1->val;
                p1 = p1->next;
            }
            if (p2) {
                val += p2->val;
                p2 = p2->next;
            }
            carry = val / 10;
            val = val % 10;
            p->next = new ListNode(val);
            p = p->next;
        }
        return dummy->next;
    }
};
```
#### 21 Merge Two Sorted Lists
```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode *p1 = list1, *p2 = list2, *dummy = new ListNode(-1);
        ListNode *p = dummy;
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
```

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
```cpp
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

LeetCode 07/23/22 Weekly Contest03 (2353)
[link](https://leetcode.com/contest/weekly-contest-303/problems/design-a-food-rating-system/)
```cpp
struct cmp{
    bool operator() (pair<int, string>& p1, pair<int, string>& p2) {
        if (p1.first == p2.first) return p1.second > p2.second;
        return p1.first < p2.first;
    }  
};

class FoodRatings {
private:
    unordered_map<string, priority_queue<pair<int, string>, vector<pair<int, string>>, cmp>> cuisine_food;
    unordered_map<string, int> food_rating;
    unordered_map<string, string> food_cuisine;
public:
    FoodRatings(vector<string>& foods, vector<string>& cuisines, vector<int>& ratings) {
        for (int i = 0; i < foods.size(); i++) {
            cuisine_food[cuisines[i]].emplace(ratings[i], foods[i]);
            food_rating[foods[i]] = ratings[i];
            food_cuisine[foods[i]] = cuisines[i];
        }
    }
    
    void changeRating(string food, int newRating) {
        food_rating[food] = newRating;
        cuisine_food[food_cuisine[food]].emplace(newRating, food);
    }
    
    string highestRated(string cuisine) {
        while (food_rating[cuisine_food[cuisine].top().second] != cuisine_food[cuisine].top().first) {
            cuisine_food[cuisine].pop();
        }
        return cuisine_food[cuisine].top().second;
    }
};

/**
 * Your FoodRatings object will be instantiated and called as such:
 * FoodRatings* obj = new FoodRatings(foods, cuisines, ratings);
 * obj->changeRating(food,newRating);
 * string param_2 = obj->highestRated(cuisine);
 */

// 07/24/22 First Time
// We can make a map<cuisine, food>, map<food, rating>, and find a max rating using for loop each time, but this will cause time exceed. We can also design a priority_queue in cuisine, each time we update a rating, we push this new rating with food name into priority queue, and when we call highestrated, we can just check if this top {rating, food} is still existing or not. If not, we just pop out top and check another one. Note this problem does not need us to delete changed food rating, so we can do this, otherwie this method is not working. C++ Priority queue does not have a remove function, so it is not easy to delete from priority queue. This problem also make each food have only one rating, so this is convinient for us to check if the food rating is valid or not.
// Time Complexity: O(NlogN) for priority queue in foodRatings function, and O(1) in highestRated, O(logN) in changeRating. 
```
# Sort PQ with pair comparation
LeetCode 692. Top K Frequent Words
```cpp
class Solution {
private:
    struct cmp {
        bool operator() (pair<int, string> a, pair<int, string> b) {
            if (a.first == b.first) {
                return a.second > b.second;
            }
            else {
                return a.first < b.first;
            }
        }
    };
public:
    vector<string> topKFrequent(vector<string>& words, int k) {
        unordered_map<string, int> map;
        for (string& word: words) map[word]++;
        priority_queue<pair<int, string>, vector<pair<int, string>>, cmp> pq;
        for (auto it: map) {
            pq.emplace(it.second, it.first);
        }
        vector<string> res;
        while (res.size() < k) {
            res.push_back(pq.top().second);
            pq.pop();
        }
        return res;
    }
};

// 10/18/22 First Time
// First we need to find the frequency of each string, then we need to sort strings by frequency and lexicographical order (if frequency same), then we find the top k frequent and smaller lexicographical (if kth and k+1th has the same frequency), then return the top kth strings to result vector. The only thing to notice is that the default pq is sorting elements in "less than" order (if there is a smaller element pushed into pq, it will be put at bottom to make it seems like newly pushed into a normal queue, and larger element will thus float up to the top), so it will make smaller lexicographical string deeper, and thus we will get larger lexicographical strings and not in desired prder, so we need to write our own comparator: if pair a.first<b.first, this is normal "default" pq setting (if a.first <b.first==true, then a will be put deeper and b will float upper, so smaller frequency will be at bottom). If a.first==b.first, we want smaller lexicographical string to float up so that we will pop them first into result array, so we need to return a.second>b.second, which means we will put a deeper and b upper if a.first==b.first and a.second>b.second==true
// Time Complexity: O(NlogN)
```

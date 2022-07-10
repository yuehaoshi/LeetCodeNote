# Hash Map (Set)
## Hash Map C++ STL
Unordered_map, implemented with hash map in c++, is different than map:
[Reference Here](https://www.geeksforgeeks.org/map-vs-unordered_map-c/#:~:text=map%20is%20used%20to%20store,pairs%20in%20non%2Dsorted%20order.)
|                | map                                    | unordered_map                      |
|----------------|----------------------------------------|------------------------------------|
|Ordering        | increasing  order (by default)         | no ordering                        |
|Implementation  | Self balancing BST like Red-Black Tree | Hash Table                         |
|search time     | log(n)                                 | O(1) -> Average<br>O(n) -> Worst Case |
|Insertion time  | log(n) + Rebalance                     | Same as search                     |
|Deletion time   | log(n) + Rebalance                     | Same as search                     |

### Methods
Initialization:
```cpp
unordered_map<string, int> umap;
```
Insertion:
```cpp
umap["a"] = 20;
OR
umap.insert(make_pair("e", 2.718));
```
Find:
```cpp
if (umap.find(key) == umap.end())
    cout << key << " not found\n\n";
OR
auto it = m.find('e');  
if ( it == m.end() ) 
    cout<<"Element not found"; 
```
Iterator:
```cpp
unordered_map<string, int>:: iterator p;
for (p = wordFreq.begin(); p != wordFreq.end(); p++)
    cout << "(" << p->first << ", " << p->second << ")\n";
OR
for (auto i : m)
    cout << i.first << "   " << i.second<< endl;
```
### Some Operation:
Put a sting into a map:
```cpp
map<int, int> m;
for (int i = 0; i < n; i++)
    m[arr[i]]++;
```
Reference: https://www.geeksforgeeks.org/traversing-a-map-or-unordered_map-in-cpp-stl/

Example:
check if s2 contains s1: (Simplified from Leetcode 567)
```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        unordered_map<char, int> map1;
        unordered_map<char, int> map2;
        for (auto c1: s1) map1[c1]++;
        for (auto c2: s2) map2[c2]++;
        for (auto it: map1) {
            if (map2.find(it.first) == map2.end()) return false;
            else {
                auto it2 = map2.find(it.first);
                if (it2->second < it.second) return false;
            }
        }
        return true;
    }
};
```
Another Example:<br>
map.count, map[key] difference<br>
LeetCode 76 Minimum Window Substring:<br>
```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> needed, window;
        for (char c: t) needed[c]++;
        int left = 0, right = 0;
        int valid = 0;
        int start = 0, len = s.size() + 1;
        while (right < s.size()) {
            if (needed.count(s[right])) {
                //Cannot use needed[s[right]] in if() here, because if s[right] is not in needed, needed will add a key s[right] with null value;
                cout<<needed[s[right]]<<endl;
                window[s[right]]++;
                if (window[s[right]] == needed[s[right]]) valid++;
            }
            while (valid == needed.size()) {
                if (right + 1 - left < len) {
                    start = left;
                    len = right + 1 - left;
                }
                if (needed.count(s[left])) {
                    if (window[s[left]] == needed[s[left]]) valid--;
                    window[s[left]]--;
                }
                left++;
            }
            right++;
        }
        if (len == s.size() + 1) return "";
        return s.substr(start, len);
    }
};
```
Sometimes if we use hashmap for recording indexes, for example, record current order before aorting an array, we can use vector<pair<int, int>> instead.
For example: 
Leetcode 870
```cpp
class Solution {
public:
    vector<int> advantageCount(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size();
        sort(nums1.begin(), nums1.end());
        vector<pair<int, int>> map2;
        for (int i = 0; i < n; i++) map2.push_back(make_pair(nums2[i], i));
        sort(map2.begin(), map2.end());
        vector<int> res(n);
        int left = 0, right = n - 1;
        for (int i = n - 1; i >= 0; i--) {
            if (nums1[right] > map2[i].first) {
                res[map2[i].second] = nums1[right];
                right--;
            }
            else {
                res[map2[i].second] = nums1[left];
                left++;
            }
        }
        return res;
    }
};
```
## Hash Map Examples
#### 705 Design HashSet
```cpp
class MyHashSet {
private:
    vector<list<int>> data;
    static const int base = 769;
    static int hash(int key) {
        return key % base;
    }
public:
    MyHashSet(): data(base) {}
    
    void add(int key) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); it++) {
            if ((*it) == key) return;
        }
        data[h].push_back(key);
    }
    
    void remove(int key) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); it++) {
            if ((*it) == key) {
                data[h].erase(it);
                return;
            }
        }
    }
    
    bool contains(int key) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); it++) {
            if ((*it) == key) return true;
        }
        return false;
    }
};

/**
 * Your MyHashSet object will be instantiated and called as such:
 * MyHashSet* obj = new MyHashSet();
 * obj->add(key);
 * obj->remove(key);
 * bool param_3 = obj->contains(key);
 */
```
#### 706 Design HashMap
```cpp
class MyHashMap {
private:
    vector<list<pair<int, int>>> data;
    static const int base = 769;
    static int hash(int key) {
        return key % base;
    }
public:
    MyHashMap(): data(base) {}
    
    void put(int key, int value) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); it++) {
            if ((*it).first == key) {
                (*it).second = value;
                return;
            }
        }
        data[h].push_back(make_pair(key, value));
    }
    
    int get(int key) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); it++) {
            if ((*it).first == key) return (*it).second;
        }
        return -1;
    }
    
    void remove(int key) {
        int h = hash(key);
        for (auto it = data[h].begin(); it != data[h].end(); it++) {
            if ((*it).first == key) {
                data[h].erase(it);
                return;
            }
        }
    }
};

/**
 * Your MyHashMap object will be instantiated and called as such:
 * MyHashMap* obj = new MyHashMap();
 * obj->put(key,value);
 * int param_2 = obj->get(key);
 * obj->remove(key);
 */
```

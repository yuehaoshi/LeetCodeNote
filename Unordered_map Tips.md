Unordered_map, implemented with hash map in c++, is different than map:
[Reference Here](https://www.geeksforgeeks.org/map-vs-unordered_map-c/#:~:text=map%20is%20used%20to%20store,pairs%20in%20non%2Dsorted%20order.)
|                | map                                    | unordered_map                      |
|----------------|----------------------------------------|------------------------------------|
|Ordering        | increasing  order (by default)         | no ordering                        |
|Implementation  | Self balancing BST like Red-Black Tree | Hash Table                         |
|search time     | log(n)                                 | O(1) -> Average<br>O(n) -> Worst Case |
|Insertion time  | log(n) + Rebalance                     | Same as search                     |
|Deletion time   | log(n) + Rebalance                     | Same as search                     |

Initialization:
```
unordered_map<string, int> umap;
```
Insertion:
```
umap["a"] = 20;
OR
umap.insert(make_pair("e", 2.718));
```
Find:
```
if (umap.find(key) == umap.end())
    cout << key << " not found\n\n";
OR
auto it = m.find('e');  
if ( it == m.end() ) 
    cout<<"Element not found"; 
```
Iterator:
```
unordered_map<string, int>:: iterator p;
for (p = wordFreq.begin(); p != wordFreq.end(); p++)
    cout << "(" << p->first << ", " << p->second << ")\n";
OR
for (auto i : m)
    cout << i.first << "   " << i.second<< endl;
```
Some Operation:
Put a sting into a map:
```
map<int, int> m;
for (int i = 0; i < n; i++)
    m[arr[i]]++;
```
Reference: https://www.geeksforgeeks.org/traversing-a-map-or-unordered_map-in-cpp-stl/

Example:
check if s2 contains s1: (Simplified from Leetcode 567)
```
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
Another Example:
map.count, map[key] difference
LeetCode 76 Minimum Window Substring:
```
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

Unordered_map, implemented with hash map in c++, is different than map:
                | map                 | unordered_map
---------------------------------------------------------
Ordering        | increasing  order   | no ordering
                | (by default)        |

Implementation  | Self balancing BST  | Hash Table
                | like Red-Black Tree |  

search time     | log(n)              | O(1) -> Average 
                |                     | O(n) -> Worst Case

Insertion time  | log(n) + Rebalance  | Same as search
                      
Deletion time   | log(n) + Rebalance  | Same as search

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

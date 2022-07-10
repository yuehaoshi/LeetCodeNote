# BFS
### 297 Serialize and Deserialize Binary Tree
Solution1: DFS (recursion in pre-order)
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    void traverse(TreeNode* root, string& str) {
        if (root == NULL) str += "#,";
        else {
            str += to_string(root->val) + ",";
            traverse(root->left, str);
            traverse(root->right, str);
        }
    }
    
    string serialize(TreeNode* root) {
        string str;
        traverse(root, str);
        return str;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        queue<string> que; // que can better pop put front
        string temp;
        for (char c: data) {
            if (c == ',') {
                que.emplace(temp);
                temp.clear();
            }
            else temp.push_back(c);
        }
        return build(que);
    }
    
    TreeNode* build(queue<string>& que) {
        if (que.front() == "#") {
            que.pop();
            return NULL;
        }
        TreeNode* root = new TreeNode(stoi(que.front()));
        que.pop();
        root->left = build(que);
        root->right = build(que);
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```
Solution2: BFS
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    void traverse(TreeNode* root, string& str) {
        queue<TreeNode*> que;
        que.push(root);
        //str += to_string(root->val) + ",";
        while (!que.empty()) {
            int size = que.size();
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                if (cur == NULL) str += "#,";
                else {
                    str += to_string(cur->val) + ",";
                    que.push(cur->left);
                    que.push(cur->right);
                }
            }
        }
    }
    
    string serialize(TreeNode* root) {
        string str;
        traverse(root, str);
        return str;
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        queue<string> str; // queue can better pop put front
        string temp;
        for (char c: data) {
            if (c == ',') {
                str.emplace(temp);
                temp.clear();
            }
            else temp.push_back(c);
        }
        return build(str);
    }
    
    TreeNode* build(queue<string>& str) {
        if (str.front() == "#") return NULL;
        TreeNode* root = new TreeNode(stoi(str.front()));
        queue<TreeNode*> que;
        que.push(root);
        str.pop();
        while (!que.empty() && !str.empty()) {
            TreeNode* cur = que.front();
            que.pop();
            if (str.front() != "#") {
                TreeNode* left = new TreeNode(stoi(str.front()));
                cur->left = left;
                que.push(left);
                str.pop();
            }
            else {
                cur->left = NULL;
                str.pop();
            }
            if (str.front() != "#") {
                TreeNode* right = new TreeNode(stoi(str.front()));
                cur->right = right;
                que.push(right);
                str.pop();
            }
            else {
                cur->right = NULL;
                str.pop();
            }
        }
        return root;
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```
### 210 Course Schedule II
There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.
For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.
Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.
> Example 1:  
Input: numCourses = 2, prerequisites = [[1,0]]  
Output: [0,1]  
Explanation: There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is [0,1].  

> Example 2:  
Input: numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]  
Output: [0,2,1,3]  
Explanation: There are a total of 4 courses to take. To take course 3 you should have finished both courses 1 and 2. Both courses 1 and 2 should be taken after you finished course 0.  
So one correct course order is [0,1,2,3]. Another correct ordering is [0,2,1,3].  

> Example 3:  
Input: numCourses = 1, prerequisites = []  
Output: [0]  

This is a classic topological graph algorithm, solution here: 
https://leetcode.cn/problems/course-schedule-ii/solution/ke-cheng-biao-ii-by-leetcode-solution/

Solution 1: BFS
```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> edges(numCourses);
        vector<int> indeg(numCourses); 
        //Indegree means the number of prerequisites of current course
        vector<int> result;
        queue<int> que;
        for (const auto& pre: prerequisites) {
            edges[pre[1]].push_back(pre[0]);
            //edges[i] means all courses that take course i as prereq.
            indeg[pre[0]]++;
            //indeg[i] means how many prereqs for course i.
        }
        for (int i = 0; i < indeg.size(); i++) {
            if (indeg[i] == 0) que.push(i);
        }
        while (!que.empty()) {
            int u = que.front();
            //u is the course to be taken this time
            que.pop();
            result.push_back(u);
            for (int v: edges[u]) {
                //v are the courses that take u as prereq.
                indeg[v]--;
                if (indeg[v] == 0) que.push(v);
            }
        }
        if (result.size() < numCourses) return {};
        return result;
    }
};
```
Solution 2: DFS
```cpp
```
### 200 Number of Islands
Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.
An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

Solution 1: DFS
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int res = 0;
        vector<vector<int>> visited(grid.size(), vector<int>(grid[0].size(), 0));
        //use visited matrix to avoid change original matrix
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[0].size(); j++) {
                if (grid[i][j] == '1' && visited[i][j] == 0) {
                    //Note: grid[i][j] ==. '1' but not 1
                    dfs(grid, i, j, visited);
                    res++;
                }
            }
        }
        return res;
    }
    void dfs(vector<vector<char>>& grid, int i, int j, vector<vector<int>>& visited) {
        if (i < 0 || j < 0 || i >= grid.size() || j >= grid[0].size()) return;
        //Donot forget those boundary cases
        if (visited[i][j] == 1 || grid[i][j] == '0') return;
        visited[i][j] = 1;
        dfs(grid, i + 1, j, visited);
        dfs(grid, i, j + 1, visited);
        dfs(grid, i - 1, j, visited);
        dfs(grid, i, j - 1, visited);
    }
};
```
Solution 2: BFS
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int res = 0;
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[0].size(); j++) {
                if (grid[i][j] == '1') {
                    res++;
                    grid[i][j] = '0';
                    queue<pair<int, int>> neighbors;
                    neighbors.push({i, j});
                    while (!neighbors.empty()) {
                        auto cur = neighbors.front();
                        neighbors.pop();
                        int r = cur.first, c = cur.second;
                        if (r + 1 < grid.size() && grid[r + 1][c] == '1') {
                            neighbors.push({r + 1, c});
                            grid[r + 1][c] = '0';
                        }
                        if (c + 1 < grid[0].size() && grid[r][c + 1] == '1') {
                            neighbors.push({r, c + 1});
                            grid[r][c + 1] = '0';
                        }
                        if (r - 1 >= 0 && grid[r - 1][c] == '1') {
                            neighbors.push({r - 1, c});
                            grid[r - 1][c] = '0';
                        }
                        if (c - 1 >= 0 && grid[r][c - 1] == '1') {
                            neighbors.push({r, c - 1});
                            grid[r][c - 1] = '0';
                        }
                    }
                }
            }
        }
        return res;
    }
};
```
Solution 3: Union-Find
```
```
### 133 Clone Graph
Given a reference of a node in a connected undirected graph. Return a deep copy (clone) of the graph.
Each node in the graph contains a value (int) and a list (List[Node]) of its neighbors.
```cpp
// Definition for a Node.
class Node {
public:
    int val;
    vector<Node*> neighbors;
    Node() {
        val = 0;
        neighbors = vector<Node*>();
    }
    Node(int _val) {
        val = _val;
        neighbors = vector<Node*>();
    }
    Node(int _val, vector<Node*> _neighbors) {
        val = _val;
        neighbors = _neighbors;
    }
};
```
Solution 1: DFS
```cpp
class Solution {
public:
    unordered_map<Node*, Node*> visited;
    //visited map contains each node and its clone node
    Node* cloneGraph(Node* node) {
        if (node == nullptr) return node; 
        if (visited.find(node) != visited.end()) return visited[node];
        Node* clone = new Node(node->val);
        visited[node] = clone;
        for (auto& neighbor: node->neighbors) {
            clone->neighbors.push_back(cloneGraph(neighbor));
            //clone node's neighbors are the cloned neighbors
        }
        return clone;
    }
};
```
Solution 2: BFS
```cpp
class Solution {
public:
    Node* cloneGraph(Node* node) {
        if (node == nullptr) return node;
        unordered_map<Node*, Node*> visited;
        //visited stores visited and cloned nodes
        queue<Node*> que;
        que.push(node);
        visited[node] = new Node(node->val);
        while (!que.empty()) {
            auto cur = que.front();
            que.pop();
            for (auto& neighbor: cur->neighbors) {
                //Iterate all neighbors of current node
                if (visited.find(neighbor) == visited.end()) {
                    visited[neighbor] = new Node(neighbor->val);
                    que.push(neighbor);
                    //Every time we visit a new node and create a cloned node, remember to add it to queue for future visit
                }
                visited[cur]->neighbors.push_back(visited[neighbor]);
                //Connect curretn cloned node with its neighbors
            }
        }
        return visited[node];
    }
};
```

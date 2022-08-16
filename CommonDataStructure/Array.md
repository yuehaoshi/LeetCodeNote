[LeetCode 36](https://leetcode.com/problems/valid-sudoku/)

```cpp
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        int n = board.size();
        int box[3][3][9];
        int rows[9][9];
        int cols[9][9];
        memset(box, 0, sizeof(box));
        memset(rows, 0, sizeof(rows));
        memset(cols, 0, sizeof(cols));
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] != '.') {
                    int d = board[i][j] - '1';
                    if (box[i / 3][j / 3][d] || rows[i][d] || cols[j][d]) return false;
                    box[i / 3][j / 3][d]++;
                    rows[i][d]++;
                    cols[j][d]++;
                }
            }
        }
        return true;
    }
};

// 07/27/22 Second TIme
// Traverse once, but pretty slow. Optimized idea is: use array but not vector, and use array but not hash set
```

Performance compared to vector:  
- Vector is better for frequent insertion and deletion, whereas Arrays are much better suited for frequent access of elements scenario.  
- Vector occupies much more memory in exchange for managing storage and growing dynamically, whereas Arrays are a memory-efficient data structure.  
[Reference](https://www.educba.com/c-plus-plus-vector-vs-array/)

C++ array Initialize with value:
```
int arr[26];
fill_n(arr, 26, 0);
cout<<"arr[0]: "<<arr[0]<<endl;
cout<<"size of arr: "<<sizeof(arr)<<endl;
cout<<"size of arr: "<<sizeof(arr) / sizeof(int)<<endl;
```
output is:  
"  
arr[0]: 0  
size of arr: 104  
size of arr: 26  
"  

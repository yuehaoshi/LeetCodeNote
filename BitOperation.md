## Exclusive Or Operation
Exclusive Or Operation:<br>
a⊕0=a<br>
a⊕a=0<br>
a⊕b⊕a=b⊕a⊕a=b⊕(a⊕a)=b⊕0=b<br>
So:<br>
(a1⊕a1)⊕(a2⊕a2)⊕⋯⊕(am⊕am)⊕am+1 = 0⊕0⊕⋯⊕0⊕am+1=am+1

### 136 Single Number
```
int singleNumber(vector<int>& nums) {
        int res = 0;
        for (int n: nums) res ^= n;
        return res;
    }
```
### 268 Missing Number
```
int missingNumber(vector<int>& nums) {
        int res = nums.size();
        for (int i = 0; i < nums.size(); i++) res ^= i ^ nums[i];
        return res;
    }
```

## n & (n-1)
Delete the last "1" in binary number
### 231 Power of Two
```
bool isPowerOfTwo(int n) {
        if (n <= 0) return false;
        return (n & (n - 1)) == 0;
    }
```

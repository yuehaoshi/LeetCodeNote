```
ERROR: AddressSanitizer: stack-buffer-overflow on address...
```
This error could because of using "=" when need using "=="

An interesting bug:
while writing a for loop, I firstly used "vec.size()" directly:
```
for (int i = vec.size() - 1; i >= vec.size() - k; i--) {
            res.push_back(vec[i].second);
        }
```
It raised an error: 
```
runtime error: addition of unsigned offset 0x.... overflow to 0x....
```
But when I changed to the following code, it runs well:
```
int n = vec.size();
        for (int i = n - 1; i >= n - k; i--) {
            res.push_back(vec[i].second);
        }
```
It turns out that size() is an unsigned value, and it will be a problem in "i>=vec.size() - k"
(Leetcode 347)
https://jdhao.github.io/2017/10/07/loop-forward-backward-with-cpp-vector/

Difference between Emplace and Push (for stack, priority_queue):
Emplace is creating the to be inserted element, push is just to push the copy.<br>
Emplace can use the inner constructor for its input elements, eg
```
priority_queue<pair<int, int>> pq;
pq.emplace(1, 2)
//This line is equal to
pq.push(make_pair(1, 2))
```

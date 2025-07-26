## 6. Z字形变换
将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行 Z 字形排列。
### 代码
```bash
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows < 2)
            return s;
        vector<string> rows(numRows);
        int i = 0, flag = -1;
        for(char c : s){
            rows[i].push_back(c);
            if( i == 0 || i == numRows - 1)
                flag = -flag;
            i += flag;
        }
        string res;
        for (const string &row : rows)
            res += row;
        return res;

    }
};
```
## 18. 四数之和
给你一个由 n 个整数组成的数组 nums ，和一个目标值 target 。请你找出并返回满足下述全部条件且不重复的四元组 [nums[a], nums[b], nums[c], nums[d]] （若两个四元组元素一一对应，则认为两个四元组重复）：
- 0 <= a, b, c, d < n
- abcd互不相同
- nums[a] + nums[b] + nums[c] + nums[d] == target
### 代码
```bash
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> quadruplets;
        if (nums.size() < 4){
            return quadruplets;
        }
        sort(nums.begin(), nums.end());
        int length = nums.size();
        for(int i = 0; i < length - 3; i++){
            if(i > 0 && nums[i] == nums[i - 1]){
                continue;
            }
            for(int j = i + 1; j < length - 2; j++){
                if(j > i + 1 && nums[j] == nums[j - 1]){
                    continue;
                }
                int left = j + 1, right = length - 1;
                while(left < right){
                    long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];
                    if(sum == target){
                                              quadruplets.push_back({nums[i], nums[j], nums[left], nums[right]});
                        while (left < right && nums[left] == nums[left + 1]) {
                            left++;
                        }
                        left++;
                        while (left < right && nums[right] == nums[right - 1]) {
                            right--;
                        }
                        right--;
                    } else if (sum < target) {
                        left++;
                    } else {
                        right--;
                    }
                    }
                }
            }
        return quadruplets;
        }
   
    
};
```

### 思路
使用上述方法，可以避免枚举到重复四元组，但是由于仍使用四重循环，时间复杂度仍是 O(n4)。注意到数组已经被排序，因此可以使用双指针的方法去掉一重循环。

使用两重循环分别枚举前两个数，然后在两重循环枚举到的数之后使用双指针枚举剩下的两个数。假设两重循环枚举到的前两个数分别位于下标 i 和 j，其中 i<j。初始时，左右指针分别指向下标 j+1 和下标 n−1。每次计算四个数的和，并进行如下操作：

如果和等于 target，则将枚举到的四个数加到答案中，然后将左指针右移直到遇到不同的数，将右指针左移直到遇到不同的数；

如果和小于 target，则将左指针右移一位；

如果和大于 target，则将右指针左移一位。

使用双指针枚举剩下的两个数的时间复杂度是 O(n)，因此总时间复杂度是 O(n3)，低于 O(n4)。

## 77. 组合
给定两个整数n和k，返回范围[1, n]中所有可能的k个数的组合
### 代码（不考虑剪枝）
```bash
vector<int> temp;
void dfs(int cur, int n, int k) {
    // 记录合法答案
    if (temp.size() == k) {
        ans.push_back(temp);
        return;
    }

    // 到底了，但组合不满，不是合法组合
    if (cur > n) {
        return;
    }

    // 选择当前位置
    temp.push_back(cur);
    dfs(cur + 1, n, k);
    temp.pop_back();

    // 不选择当前位置
    dfs(cur + 1, n, k);
}
```
### 代码（考虑剪枝）
```bash
class Solution {
public:
    vector<int> temp;
    vector<vector<int>> ans;
    void dfs(int cur, int n, int k){
        if(temp.size() + (n - cur + 1) < k){
            return;
        }
        if(temp.size() == k){
            ans.push_back(temp);
            return;
        }
        temp.push_back(cur);
        dfs(cur + 1, n, k);
        temp.pop_back();
        dfs(cur + 1, n, k);
    }
    vector<vector<int>> combine(int n, int k){
        dfs(1, n, k);
        return ans;   
    }
};
```
### 思路
从 n 个当中选 k 个的所有方案对应的枚举是组合型枚举。在中我们用递归来实现组合型枚举。

首先我们先回忆一下如何用递归实现二进制枚举（子集枚举），假设我们需要找到一个长度为 n 的序列 a 的所有子序列，代码框架是这样的：
```bash
vector<int> temp;
void dfs(int cur, int n) {
    if (cur == n + 1) {
        // 记录答案
        // ...
        return;
    }
    // 考虑选择当前位置
    temp.push_back(cur);
    dfs(cur + 1, n, k);
    temp.pop_back();
    // 考虑不选择当前位置
    dfs(cur + 1, n, k);
}
```


这个时候我们可以做一个剪枝，如果当前 temp 的大小为 s，未确定状态的区间 [cur,n] 的长度为 t，如果 s+t<k，那么即使 t 个都被选中，也不可能构造出一个长度为 k 的序列，故这种情况就没有必要继续向下递归，即我们可以在每次递归开始的时候做一次这样的判断：

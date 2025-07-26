## 3. 无重复字符的最长字串
给定一个字符串 s ，请你找出其中不含有重复字符的**最长子串**的长度。

### 思路
使用了双指针或滑动窗口的思想
- 使用两个指针表示字符串中的某个子串（或窗口）的左右边界
- 在每一步的操作中，我们会将左指针向右移动一格，表示我们开始枚举下一个字符作为起始位置。
- 此外，我们还需要使用一个数据结构来判断是否有重复的字符，常用的数据结构为哈希集合（即 C++ 中的 std::unordered_set，Java 中的 HashSet，Python 中的 set, JavaScript 中的 Set）

### 代码
```bash
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
       unordered_set<char> occ;
       int n = s.size();
       int rk = -1, ans = 0;
       for (int i = 0; i < n ; ++i){
            if(i != 0)
            {
                occ.erase(s[i - 1]);
            }
            while (rk + 1 < n && !occ.count(s[rk + 1])){
                occ.insert(s[rk + 1]);
                ++rk;
            }
            ans = max(ans, rk - i + 1);
        
       } 
       return ans;
    }
};
```

## 5. 最长回文字串
给你一个字符串 s，找到 s 中最长的回文子串。
```bash
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        if (n < 2){
            return s;
        }
        int maxLen = 1;
        int begin = 0;
        vector<vector<int>> dp(n, vector<int>(n));
        for (int i = 0; i < n; i++){
            dp[i][i] = true;
        }
        for (int L = 2; L <= n; L++){
            for(int i = 0 ; i < n ; i++){
                int j = L + i - 1;
                if(j >= n){
                    break;
                }
                if(s[i] != s[j]){
                    dp[i][j] = false;
                }
                else{
                    if(j - i < 3){
                        dp[i][j] = true;
                    }
                    else{
                        dp[i][j] = dp[i + 1][j - 1];
                    }
                }
                if (dp[i][j] && j - i + 1 > maxLen){
                    maxLen = j - i + 1;
                    begin = i;
                }

            }

        }
        return s.substr(begin, maxLen);
    }
};
```

### 思路
- 动态规划的状态转移方程
  $P(i,j)=P(i+1,j−1)∧(S_i == S_j)$
- 动态规划中的边界条件（字串长度为1或2）

---
> **Notice**<br>
在状态转移方程中，我们是从长度较短的字符串向长度较长的字符串进行转移的，因此一定要注意动态规划的循环顺序<br>
---

## 16. 最接近的三数之和
给你一个长度为 n 的整数数组 nums 和 一个目标值 target。请你从 nums 中选出三个整数，使它们的和与 target 最接近。
返回这三个数的和。
### 代码

```bash
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        int best = 1e7;
        auto update = [&](int cur){
            if(abs(cur - target) < abs(best - target)){
                best = cur;
            }
        };
        for(int i = 0; i < n; ++i){
            if(i > 0 && nums[i] == nums[i - 1]){
                continue;
            }
            int j = i + 1, k = n - 1;
            while( j < k){
                int sum = nums[i] + nums[j] + nums[k];
                if(sum == target){
                    return target;
                } 
                update(sum);
                if(sum > target){
                    k--;
                }else{
                    j++;
                }


            }
        }
        return best;
    }
};
```
### 思路
排序+双指针
借助双指针，我们就可以对枚举的过程进行优化。我们用 $p_b$和 $p_c$分别表示指向 b 和 c 的指针，初始时，p b 指向位置 i+1，即左边界；p c
指向位置 n−1，即右边界。在每一步枚举的过程中，我们用 a+b+c 来更新答案，并且：

如果 a+b+c≥target，那么就将 $p_c$向左移动一个位置
如果 a+b+c<target，那么就将 $p_b$向右移动一个位置。




## 3. 无重复字符的最长字串
给定一个字符串 s ，请你找出其中不含有重复字符的**最长子串**的长度。

## 思路
使用了双指针或滑动窗口的思想
- 使用两个指针表示字符串中的某个子串（或窗口）的左右边界
- 在每一步的操作中，我们会将左指针向右移动一格，表示我们开始枚举下一个字符作为起始位置。
- 此外，我们还需要使用一个数据结构来判断是否有重复的字符，常用的数据结构为哈希集合（即 C++ 中的 std::unordered_set，Java 中的 HashSet，Python 中的 set, JavaScript 中的 Set）

## 代码
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

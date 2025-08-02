## 14. 最长公共前缀
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。


### 代码
```bash
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        if(!strs.size()){
            return "";
        }
        string prefix = strs[0];
        int count = strs.size();
        for(int i = 1; i < count ; ++i){
            prefix = longestCommonPrefix(prefix, strs[i]);
            if(!prefix.size()){
                break;
            }
        }
        return prefix;
    }
    string longestCommonPrefix(const string& str1, const string& str2){
        int length = min(str1.size(), str2.size());
        int index = 0;
        while(index < length && str1[index] == str2[index]){
            index++;
        }
        return str1.substr(0,index);
    }

};
```

### 思路
横向扫描
用 LCP（SL..Sn）表示字符串 S_..S，的最长公共前缀。
可以得到以下结论：
LCP(S1. Sn) = LCP(LCP(LCP(S1, S2), S3),... Sn)
基于该结论，可以得到一种查找字符串数组中的最长公共前缀的简单方法。依次遍历字符串数组中的每个字符串，对于每个遍历到的字符串，更新最长公共前缀，当遍历完所有的字符串以后，即可得到字符串数组中的最长公共前

## 24. 两两交换链表中的节点
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
### 代码
```bash
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
      if(head == nullptr || head->next == nullptr){
        return head;
      }  
      ListNode* newHead = head->next;
      head->next = swapPairs(newHead->next);
      newHead->next = head;
      return newHead;
    }
};
```
### 思路
可以通过递归的方式实现两两交换链表中的节点。

递归的终止条件是链表中没有节点，或者链表中只有一个节点，此时无法进行交换。

如果链表中至少有两个节点，则在两两交换链表中的节点之后，原始链表的头节点变成新的链表的第二个节点，原始链表的第二个节点变成新的链表的头节点。链表中的其余节点的两两交换可以递归地实现。在对链表中的其余节点递归地两两交换之后，更新节点之间的指针关系，即可完成整个链表的两两交换。

## 46. 全排列
给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

### 代码
```bash
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        dfs(nums, 0);
        return res;
    }
private:
    vector<vector<int>> res;
    void dfs(vector<int> nums, int x){
        if(x == nums.size() - 1){
            res.push_back(nums);
            return;
        }
        for(int i = x; i < nums.size(); i++){
            swap(nums[i], nums[x]);
            dfs(nums, x + 1);
            swap(nums[i], nums[x]);
        }
    }
};
```
### 思路

使用swap来进行操作
x=0的时候，i从0到en-1遍历，也就是所有元素都被拿来当作一次第0个元素
x=1的时候，i从1到en-1遍历，也就是除了第0个元素，所有元素都被拿来当作一次第1个元素
这样依次固定索引x所在的位置，x从0到len-1变化，当x==len-1的时候，就说明形成一种排列，加入结果集。

## 47. 全排列 II
给定一个可包含重复数字的序列 nums ，按任意顺序 返回所有不重复的全排列。
### 代码
```bash
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end()); // 排序后才能剪枝去重
        dfs(nums, 0, res);
        return res;
    }

    void dfs(vector<int>& nums, int index, vector<vector<int>>& res) {
        if (index == nums.size()) {
            res.push_back(nums);
            return;
        }

        unordered_set<int> used; // 当前层用过的值，用于去重
        for (int i = index; i < nums.size(); ++i) {
            if (used.count(nums[i])) continue; // 跳过同一层重复值
            used.insert(nums[i]);
            swap(nums[i], nums[index]);
            dfs(nums, index + 1, res);
            swap(nums[i], nums[index]); // 回溯
        }
    }
};

  

```
### 思路
- 每一层index固定，把所有可能出现在index的值（从index到nums.size() - 1）都换过来
- 通过swap(nums[i], nums[index])的值换到当前位
- 回溯后再换回去
- 为避免重复排列，我们用 unordered_set<int> used 来记录当前层出现过的数字，如果已经处理过这个数字，就跳过
- used.count(nums[i] == 1)表示这个数在本层已经出现过，这个机制用来剪枝，避免生成重复排列（比如多个1交换到当前位时，只做一次）

---
> **Notice**<br>
一层就是指一次DFS调用中所在的递归层数，比如dfs(0)就是第0层，dfs(1)就是第1层，以此类推<br>
---

## 48. 旋转图像
给定一个 n × n 的二维矩阵 matrix 表示一个图像。请你将图像顺时针旋转 90 度。

你必须在 原地 旋转图像，这意味着你需要直接修改输入的二维矩阵。请不要 使用另一个矩阵来旋转图像。

### 代码
```bash
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        int n = matrix.size();
        // C++ 这里的 = 拷贝是值拷贝，会得到一个新的数组
        auto matrix_new = matrix;
        for (int i = 0; i < n; ++i) {
            for (int j = 0; j < n; ++j) {
                matrix_new[j][n - i - 1] = matrix[i][j];
            }
        }
        // 这里也是值拷贝
        matrix = matrix_new;
    }
};
```

## 78.子集
给你一个整数数组 nums ，数组中的元素 互不相同 。返回该数组所有可能的子集（幂集）。

解集 不能 包含重复的子集。你可以按 任意顺序 返回解集。

### 代码
```bash
class Solution {
public:
    vector<int> t;
    vector<vector<int>> ans;
    void dfs(int cur, vector<int>& nums){
        if(cur == nums.size()){
            ans.push_back(t);
            return;
        }
        t.push_back(nums[cur]);
        dfs(cur + 1, nums);
        t.pop_back();
        dfs(cur + 1, nums);
    }
    
    vector<vector<int>> subsets(vector<int>& nums) {
       dfs(0, nums);
       return ans; 
    }
};
```
### 思路
思路和**组合问题类似**，都是DFS+回溯，当前元素选or不选，区别如下  

| 区别点    | 子集问题               | 组合问题（如 C(n,k)）           |
| ------ | ------------------ | ------------------------ |
| 是否限制长度 | ❌ 没有限制，所有长度的子集都要   | ✅ 限制组合长度为 `k`            |
| 总数     | $2^n$ 个子集          | $C(n, k)$ 个组合            |
| 示例输入   | `[1,2,3]` → 输出所有子集 | `n=4, k=2` → 输出长度为 2 的组合 |

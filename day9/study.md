## 86. 分隔链表
给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你应当 保留 两个分区中每个节点的初始相对位置。
### 代码
```
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
  ListNode* partition(ListNode* head, int x) {
  ListNode* small = new ListNode(0);
  ListNode* smallHead = small;
  ListNode* large = new ListNode(0);
  ListNode* largeHead = large;
  while(head != nullptr){
  if(head->val < x){
  small->next = head;
  small = small->next;
  }else{
  large->next = head;
  large = large->next;
  }
  head = head->next;

       }
       large->next = nullptr;
       small->next = largeHead->next;
       return smallHead->next;
  }
  };
```
### 思路
直观来说我们只需维护两个链表 small 和 large 即可，small 链表按顺序存储所有小于 x 的节点，large 链表按顺序存储所有大于等于 x 的节点。遍历完原链表后，我们只要将 small 链表尾节点指向 large 链表的头节点即能完成对链表的分隔。

为了实现上述思路，我们设 smallHead 和 largeHead 分别为两个链表的哑节点，即它们的 next 指针指向链表的头节点，这样做的目的是为了更方便地处理头节点为空的边界条件。同时设 small 和 large 节点指向当前链表的末尾节点。开始时 smallHead=small,largeHead=large。随后，从前往后遍历链表，判断当前链表的节点值是否小于 x，如果小于就将 small 的 next 指针指向该节点，否则将 large 的 next 指针指向该节点。

## 70.爬楼梯
### 代码
```bash
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 1) return 1;
        vector<int> dp(n + 1);
        dp[0] = dp[1] = 1;
        for (int i = 2; i <= n; ++i) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};

```
### 思路
自下而上（非dfs）的动态规划法


## 91.解码问题
### 代码
```bash
class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        vector<int> f(n + 1);
        f[0] = 1;
        for(int i = 1; i <= n; i++){
            if(s[i - 1] != '0'){
                f[i] += f[i-1];
            }
            if(i > 1 && s[i-2] != '0' && ((s[i-2] - '0') * 10 + (s[i - 1] - '0') <= 26)){
                f[i] += f[i - 2];
            }
        }
        return f[n];
    }
};
```
### 思路
根据最后一个或两个字符能否构成合法编码（即 '1'~'26'），有以下两种转移方式：
```bash
if (s[i - 1] != '0') {
    f[i] += f[i - 1]; // 单独解码一个字符，如 '2' -> 'B'
}
if (i > 1 && s[i - 2] != '0' && (10 * (s[i - 2]-'0') + (s[i - 1]-'0') <= 26)) {
    f[i] += f[i - 2]; // 解码两个字符，如 '12' -> 'L'
}

```

## 96. 不同的二叉搜索树
给你一个整数 n ，求恰由 n 个节点组成且节点值从 1 到 n 互不相同的 二叉搜索树 有多少种？返回满足题意的二叉搜索树的种数。

### 代码
```bash
class Solution {
public:
    int numTrees(int n) {
        vector<int> G(n + 1, 0);
        G[0] = 1;
        G[1] = 1;
        for(int i = 2; i <= n; i++){
            for(int j = 1; j <= i ;j++){
                G[i] += G[j - 1] * G[i - j];
            }
        }
        return G[n];
    }
};
```
### 题解
我们定义两个函数：
- G(n): 长度为n的序列能构成的不同二叉搜索树的个数
- F(i,n): 以i为根、序列长度为n的不同二叉搜索树个数

有以下公式：
- $G(n) = \sum_{i=1}^{n}F(i,n)$
- $F(i,n) = G(i - 1)G(n - i)$

所以状态转移方程如下：  
$G(n) = \sum_{i=1}^{n}G(i-1)G(n-i)$

## 102. 二叉树的层序遍历
给你二叉树的根节点 root ，返回其节点值的 层序遍历 。 （即逐层地，从左到右访问所有节点）。
### 代码
```bash
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> que;
        vector<vector<int>> res;
        if( root != nullptr) que.push(root);
        while(!que.empty()){
            vector<int> tmp;
            for(int i = que.size(); i > 0; i--){
                root = que.front();
                que.pop();
                tmp.push_back(root->val);
                if (root->left != nullptr) que.push(root->left);
                if (root->right != nullptr) que.push(root->right);

            }
            res.push_back(tmp);
        }
        return res;
     }
};
```
### 思路
- 特例处理： 当根节点为空，则返回空列表 [] 。
- 初始化： 打印结果列表 res = [] ，包含根节点的队列 queue = [root] 。
- BFS 循环： 当队列 queue 为空时跳出。
    - 新建一个临时列表 tmp ，用于存储当前层打印结果。 
    - 当前层打印循环： 循环次数为当前层节点数（即队列 queue 长度）。 
      - 出队： 队首元素出队，记为 node。
      - 打印： 将 node.val 添加至 tmp 尾部。 
      - 添加子节点： 若 node 的左（右）子节点不为空，则将左（右）子节点加入队列 queue 。
    - 将当前层结果 tmp 添加入 res 。
- 返回值： 返回打印结果列表 res 即可

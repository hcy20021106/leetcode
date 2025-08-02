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


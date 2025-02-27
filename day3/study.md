## 45跳跃问题2
求跳到索引为size() - 1 的位置最小的步数
### 贪心算法
将到达数组的最后一个位置的问题简化为到达最后一步跳跃前所在的位置
如果有多个位置能通过跳跃到达最后一个位置，那么可以**贪心**地选择距离最后一个位置最远的那个位置，也就是下标最小的那个位置。
```bash
class Solution {
public:
    int jump(vector<int>& nums) {
        int position = nums.size() - 1;
        int steps = 0;
        while ( position > 0  ){
            for ( int i = 0; i < position; i++){
                if(i + nums[i] >= position)
              {  
                position = i;
                steps++;
                break;
                }
            }
        }
        
        return steps;
    }
};
```

## dp动态规划

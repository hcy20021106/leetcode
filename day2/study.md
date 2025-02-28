## std::function
例如std::function<bool(int)> 表示一个类型为**bool**返回值，接受一个**int**参数的函数类型


## lambda表达式
### 格式
[捕捉对象](参数列表)mutable->return-type{statement}
### 例1
```bash
///mutable表示捕获的变量可以在Lambda内部被修改，因为不再是const类型
auto swap1 = [x,y]()mutable{
    int tmp = x;
    x = y;
    y = tmp;
};
swap2();
```
### 例2
```bash
auto swap2 = [&x,&y](){
    int tmp = x;
    x = y;
    y = tmp;

};
swap2();


```

## 55 跳跃游戏
### DFS
递归所有从起点**0**可以跳到的点，判断是否存在抵达终点**n-1**的路径
```bash
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int len = nums.size();
        function<bool(int)> dfs = [&](int i ){
            if( nums[i]+i >= len - 1  ){
                return true;
            }
            for(int j = i + 1 ; j <= min(i + nums[i], len - 2); j++){
                if(dfs(j)) return true;
            }
            return false;
        };
        return dfs(0);
    }
};
```

### 优化DFS(记忆化搜索)
通过增加一个数组**memo**,用于标记节点的状态，在递归到访问过的节点时提前返回
```bash

class Solution {
public:
    bool canJump(vector<int>& nums) {
        // 优化DFS

        int n = nums.size();
        vector<int> memo(n, 0); // 0: 未访问, 1: 能到终点, -1: 不能到终点

        // 记忆化搜索
        function<bool(int)> dfs = [&](int i) -> bool {
            if (i + nums[i] >= n - 1) return true;  // 能到终点

            // 之前访问过该点
            if (memo[i] != 0) {
                return memo[i] == 1;    // 直接返回是否能到终点
            }

            int furthestJump = min(i + nums[i], n - 1);
            for (int j = i + 1; j <= furthestJump; j++) {
                if (dfs(j)) {   //  若该分支能到终点
                    memo[i] = 1;    //  标记为【能到终点】
                    return true;
                }
            }
            memo[i] = -1; // 标记为【不能到终点】
            return false;
        };

        return dfs(0);
    }
};

```
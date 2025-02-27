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
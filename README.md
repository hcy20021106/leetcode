## SSH方法连接github远程仓库
#### 获取密钥对
```bash
ssh-keygen -t rsa - C "youremail@example.com"
```
- id_rsa.pub
这个公钥需要放到github的setting里
- id_rsa

#### 验证SSH配置是否成功
```bash
ssh -T git@github.com
```

#### 配置Git使用SSH而不是HTTPS
```bash
/// 检查远程仓库以及其连接方式
git remote -v
```

- https  

https://github.com/hcy20021106/leetcode.git
- ssh  

git@github.com:hcy20021106/leetcode.git

##### 修改方法
```bash
///使用set-url函数
git remote set-url origin git@github.com:hcy20021106/leetcode.git
```
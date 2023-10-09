# 状态



git status
### 状态简览
git status -s

### 查看更新
git diff
`git diff --cached` 查看已经暂存起来的变化


# 移除
git rm

# 移动
git mv


# 创建

# 分支

Git 的分支也非常轻量。它们只是简单地指向某个提交纪录 —— 仅此而已。所以许多 Git 爱好者传颂：


这是因为即使创建再多的分支也不会造成储存或内存上的开销，并且按逻辑分解工作到不同的分支要比维护那些特别臃肿的分支简单多了。

在将分支和提交记录结合起来后，我们会看到两者如何协作。现在只要记住使用分支其实就相当于在说：“我想基于这个提交以及它所有的 parent 提交进行新的工作。”

## 创建分支

git branch xx


## 切换分支
## 切换到其他节点
git check out XX
## 切换到Main
git checkout main

## 合并分支

git merge xx



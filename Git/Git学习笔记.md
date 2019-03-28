# Git基础
## 初次运行 Git 前的配置
1. [起步 - 初次运行 Git 前的配置](https://git-scm.com/book/zh/v1/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE)

## 获取Git仓库
有两种取得Git项目仓库的方法：
1. 在现有的目录中初始化仓库
2. 克隆现有的仓库

### 在现有的目录中初始化仓库
进入该目录并执行以下命令
```
$ git init
```
该命令将创建一个名为 .git 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。

### 克隆现有的仓库
克隆仓库的命令格式为`git clone [url]`
```
$ git clone https://github.com/GongchuangSu/HelloWeb
```
这会在当前目录下创建一个名为 “HelloWeb” 的目录，并在这个目录下初始化一个 .git 文件夹，从远程仓库拉取下所有数据放入 .git 文件夹，然后从中读取最新版本的文件的拷贝。
> 注意：初次克隆某个仓库的时候，工作目录中的所有文件都属于**已跟踪文件**，并处于未修改状态。

## 记录每次更新到仓库
工作目录下的每一个文件都不外乎这两种状态：**已跟踪**或**未跟踪**。已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于**未修改**，**已修改**或**已放入暂存区**。工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。

编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。我们逐步将这些修改过的文件放入暂存区，然后提交所有暂存了的修改，如此反复。所以使用 Git 时文件的生命周期如下：


## 撤销修改
### 丢弃工作区的修改
```git
git checkout -- file
-- 切换远程分支的正确操作
git checkout --track origin/branchName
```
> 注意：'-- file'中间的空格  
> [Git：解决head detached at xxxx](http://www.garinzhang.com/coding/55.html)

### 对指定文件恢复到指定commit
```git
git checkout <hash> <filename>
```

### 撤销提交到暂存区(unstage)
```
git reset HEAD file
```

### 撤销commit
```
1. 找到上次git commit的 id
     git log 
     找到你想撤销的commit_id

2. git reset --hard commit_id
    完成撤销,同时将代码恢复到前一commit_id 对应的版本。

3. git reset commit_id 
    完成Commit命令的撤销，但是不对代码修改进行撤销，可以直接通过git commit 重新提交对本地代码的修改。
```
## 查看修改
### 查看工作区和版本库里面最新版本的区别
```
git diff HEAD -- readme.txt
```

## 存储（stash）
```git
-- 存储
git stash
git stash save "message..."
-- 查看存储列表
git stash list
-- 取消最近一次存储
git stash pop
-- 移除指定存储
git stash drop stash@{0}
-- 应用指定存储
git stash apply stash@{2}
-- 查看指定存储内容
git stash show -p stash@{1}
```

## git rebase 和 git merge 的区别
1. [git rebase 和 git merge 的区别](https://www.jianshu.com/p/f23f72251abc)

2. [Git 分支- 分支的衍合](https://git-scm.com/book/zh-tw/v1/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E8%A1%8D%E5%90%88)

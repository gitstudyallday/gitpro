# GIT Pro笔记

## 1.GIT基础

```shell
# 设置环境参数
git config --global user.name "lys"
git config --global user.email kxxx@gmail.com

#检查配置信息
git config --list
# 或者
git config user.name

# 在现有目录初始化仓库
git init

# 从远程仓库克隆
git clone https://github.com/gitstudyallday/gitpro.git

# 把文件加入到暂存区
git add a.txt
# 提交到本地仓库
git commit -m "init"

# 不通过暂存区一次性提交到本地仓库
git commit -a -m "init"

# 检查当前仓库的状态
git status
# 简短版状态
git status -s

# 已暂存的与未暂存的文件差分
git diff
# 已暂存的跟仓库里的文件的区分
git diff --cached
git diff --staged

# 移除文件
# 先删除文件，然后从暂存区删除
rm a.txt
git rm a.txt
# 如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 -f
git rm -f a.txt


```





## 2.GIT分支

### 2.1 分支创建/切换/删除

```shell
git branch hotfix
git branch -v
git checkout hotfix

# 或者
git checkout -b hotfix

# 分支合并后可以删除
git branch -d hotfix
# 强制删除
git branch -D hotfix
```

### 2.2 合并分支到master

```shell
git checkout master
git merge hotfix

# 删除
git branch -d hotfix
```

### 2.3 推送本地分支到远程

```shell
git push origin feature/test:feature/test

git push origin [local_branch]:[remote_branch]

# remote branch 可以省略
git push origin feature/test
```

### 2.4 分支其他

```shell
# 查看分支
git branch
# 查看分支最后一次提交
git branch -v
# 查看分支的合并情况
git branch --merged
git branch --no-merged

# 查看所有的分支跟踪情况
git branch -vv

# 查看远程分支
git branch -a

# 给分支重命名
git branch -m oldName newName


```

### 2.5远程分支

```shell
#查看远程分支详细情况
git remote show origin

# 同步远程分支到本地
git fetch origin
# 合并远程分支到当前分支，尽量不要使用git pull 命令
git merge origin/serverfix

# 推送
git push -u origin master
# 如果需要把本地的分支推送到远程，跟大家共享，请参考2.3

# 跟踪分支(另起一个新分支)
git checkout -b [branch] [remotename]/[branch]
git checkout -b hotfix001 origin/hotfix
# 也可以使用下面的命令,会在本地自动创建serverfix分支
git checkout --track origin/serverfix

# 设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支
git branch -u origin/serverfix

# 删除远程分支
git push origin --delete serverfix 

# 设置本地某分支跟踪远程某分支
# 设置本地的serverfix分支跟踪远程的origin/<branch>
git branch --set-upstream-to=origin/<branch> serverfix
# 或者
git branch -u origin/<branch> serverfix

## 删除当前分支与远程分支的关联关系
git branch --unset-upstream


```

### 2.6 变基

在 Git 中整合来自不同分支的修改主要有两种方法：merge 以及 rebase 

![](a.png)

````shell
# 变基
git checkout experiment
git rebase master
````

它的原理是首先找到这两个分支（即当前分支 experiment、变基操作的目标基底分支 master）的最近共同祖
先 C2，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并存为临时文件，然后将当前分支指向目
标基底 C3, 最后以此将之前另存为临时文件的修改依序应用。 

![](b.png)



```shell
# 然后把变基后的修改，merge到master分支
git checkout master
git merge experiment

```

## 3.标签相关

```shell
# 列出标签
git tag
# 查找标签
git tag -l 'v1.8.5*'
# 创建标签(annotated)
git tag -a v1.4 -m "tag v1.4"
# 查看标签的详细情况
git show v1.4

# 创建轻量级标签
git tag v1.4.5

# 后期打标签
git log --pretty=oneline # 找到需要打标签的提交校验和
# 然后执行打标签命令
git tag -a v1.2 -m "tag for xxx" xxx

# 推送标签到remote
git push origin [tagname]
# 如果一次想推送多个tag
git push origin --tags

#本地标签删除
git tag -d v1.4

#把删除后的标签推送到远程服务器
git push origin :refs/tags/v1.4

#在特定某个标签的版本下创建分支
git checkout -b version2 v2.0
#也就是
git checkout -b [new_branch_name] [tag_name]

```

## 4.日志输出相关

```shell
# 提交历史、各个分支的指向以及项目的分支分叉情况
git log --oneline --decorate --graph --all
```

## 5. git别名

```shell
# 取消暂存文件
git config --global alias.unstage 'reset HEAD --'

git config --global alias.st status
```




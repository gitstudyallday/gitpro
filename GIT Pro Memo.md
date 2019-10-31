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

# 想把文件从 Git 仓库中删除（亦即从暂存区域移除），但仍然希望保留在当前工作目录中
git rm --cached a.txt

# 文件改名
git mv a.txt a1.txt

# 撤销提交，重新提交
git commit --amend

# 取消暂存的文件 unstage
git add CONTRIBUTING.md
git reset HEAD CONTRIBUTING.md

# 拉取最近一次提交到版本库的文件到暂存区,不改变工作区
git reset HEAD  -- <file>

# 撤销对文件的修改
# discard changes in working directory
git checkout -- <file>

# 回退版本信息
# Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
git reset --hard HEAD^

# 也可以查询版本信息
git log --pretty=oneline
# 然后回退到指定版本
git reset --hard 9e13f

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



### 2.7 远程分支改名

```sh
# 删除远程分支
git push --delete origin devel

# 重命名本地分支
git branch -m devel develop

#推送本地分支
git push origin develop
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

# 显示每次提交的差异
git log -p 
# 最近两次的差异
git log -p 2

# 查看每次提交的简略信息
git log --stat
git log --pretty=oneline
git log --pretty=format:"%h - %an, %ar : %s"

# 限制输出长度
git log --since=2.weeks



```

**git log --pretty format选项**

| 选项 | 提交对象（commit）的完整哈希字串            |
| ---- | ------------------------------------------- |
| %H   | 提交对象（commit）的完整哈希字串            |
| %h   | 提交对象的简短哈希字串                      |
| %T   | 树对象（tree）的完整哈希字串                |
| %t   | 树对象的简短哈希字串                        |
| %P   | 父对象（parent）的完整哈希字串              |
| %p   | 父对象的简短哈希字串                        |
| %an  | 作者（author）的名字                        |
| %ae  | 作者的电子邮件地址                          |
| %ad  | 作者修订日期（可以用 --date= 选项定制格式） |
| %ar  | 作者修订日期，按多久以前的方式显示          |
| %cn  | 提交者（committer）的名字                   |
| %ce  | 提交者的电子邮件地址                        |
| %cd  | 提交日期                                    |
| %cr  | 提交日期，按多久以前的方式显示              |
| %s   | 提交说明                                    |

**git log 的常用选项** 

| 选项            | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| -P              | 按补丁格式显示每个更新之间的差异。                           |
| --stat          | 显示每次更新的文件修改统计信息。                             |
| --shortstat     | 只显示 --stat 中最后的行数修改添加移除统计。                 |
| --name-only     | 仅在提交信息后显示已修改的文件清单。                         |
| --name-status   | 显示新增、修改、删除的文件清单。                             |
| --abbrev-commit | 仅显示 SHA-1 的前几个字符，而非所有的 40 个字符。            |
| --relative-date | 使用较短的相对时间显示（比如，“2 weeks ago”）。              |
| --graph         | 显示 ASCII 图形表示的分支合并历史。                          |
| --pretty        | 使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式） |



## 5. git别名

```shell
# 拉取最近一次提交到版本库的文件到暂存区  该操作不影响工作区
git config --global alias.unstage 'reset HEAD --'

git config --global alias.st status
```



## 6.设置执行权限

```shell
# 设置文件的执行权限等
git update-index --chmod +x gradlew
# 查看文件的权限
git ls-files --stage gradlew
```
## 7.Revert

假如git commit 链是
```
A -> B -> C -> D
```

如果想把B，C，D都给revert，除了一个一个revert之外，还可以使用range revert
git revert B^..D 
这样就把B,C,D都给revert了，变成：
```
A-> B ->C -> D -> D'-> C' -> B'
```

git revert OLDER_COMMIT^..NEWER_COMMIT

如果我们想把这三个revert不自动生成三个新的commit，而是用一个commit完成，可以这样：

```
git revert -n OLDER_COMMIT^..NEWER_COMMIT
git commit -m "revert OLDER_COMMIT to NEWER_COMMIT"
```

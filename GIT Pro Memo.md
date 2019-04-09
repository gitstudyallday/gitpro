# GIT Pro笔记

## 1.GIT基础





## 2.GIT分支

### 2.1 分支创建/切换

1. ```js
   git branch hotfix
   git branch -v
   git checkout hotfix
   
   或者
   git checkout -b hotfix
   
   ```

### 2.2 合并分支到master

```java
git checkout master
git merge hotfix
git branch -d hotfix
```

>>>>>>> 

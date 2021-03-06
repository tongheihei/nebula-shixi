# Git

git是目前世界上最先进的分布式版本控制系统

## Git命令使用


你的本地仓库由 git 维护的三棵“树”组成。第一个是你的 工作目录，它持有实际文件；第二个是 缓存区（Index），它像个缓存区域，临时保存你的改动；最后是 HEAD，指向你最近一次提交后的结果.

首先使用git add命令将计划的改动提交到缓存区，
然后使用git commit -m 代码提交信息"命令，将代码提交到HEAD，此时还没有提交到服务器，
执行git push origin master命令以将这些改动提交到服务器，可以把 master 换成你想要推送的任何分支。

### git log

查看提交历史，

git log --graph 可以看到分支合并图

### git reflog

查看命令历史，

### git checkout -- file

可以丢弃工作区的修改
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态

### git reset HEAD <'file'>

可以把暂存区的修改撤销掉，重新放回工作区。

### git rm -- <'file'>

可以在HEAD区删除一个file

### git remote add origin git@github.com:tongheihei/learngit.git

新增一个远程仓库，名字为ORIGIN，关联这个远程仓库

### git push -u origin master

把本地库的所有内容推送到远程库上，-u参数将本地的master分支和远程的master分支关联起来。

### git remote rm `<origin>`

解除了本地和远程的origin的绑定关系

### git clone

将一个仓库克隆到本地当前文件夹

# git分支管理

学习网址：https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424

### git checkout -b dev

创建一个名字为dev的分支，并切换到该分支

### git branch 

查看目前的分支

### git checkout master

切换分支

### git merge 'branch'

将当前分支与目标分支合并

# git merge --abort

放弃当前的merge请求

### git branch -d `<name>`

删除目标分支

### git switch -c `<name>`

创建并切换到目标分支

### git stash

保存现场

### git stash pop

回到工作现场

### git stash list 

查看现场表单

### git branch --set-upstream-to=origin/dev dev

指定本地dev分支与origin/dev分支的链接

### git pull

git pull作用是 将远程仓库中的更改合并到当前分支中

默认模式下 相当于 git fetch + git merge FETCH_HEAD 命令

# GIt

**git branch -D xxxxxx**  不能删除head 目前所在的分支

##合并

- Fast-forward merge

  ```bash
  git checkout master
  git merge feature2
  git branch -d feature2
  ```

  主分支不能有修改。

- No Fast-forward merge

  ```bash
  git checkout master
  git merge --no-ff feature2
  git branch -d feature2
  ```

  主分支可以有修改。

  # tracking branch 

  可以设置default

  # git push 需要注意的点

  Git push时，你必须要有最新的远程库变更，否则会push失败。

  # rebase

  ![image-20210618120309601](/Users/taolei/Desktop/学习记录/img/rebase.jpg)

  # Rewritting histrty
# 重写提交

git commit --amend -m "add fileC.txt"

可以更改提交，也可以更改message

# 交互式变基
```bash
git rebase -i
```


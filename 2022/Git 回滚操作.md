### Git撤销&回滚操作(git reset 和 get revert)

#### Git 的工作流

工作区：在 git add xx 之前，在自己当前分支所修改的代码内容！  
暂存区：已经 git add xxx 进去，且没有执行 git commit xxx 的。  
本地分支：已经 git commit -m xxx 提交到本地分支的。  
远程分支：git push origin HEAD:refs/for/master HEAD 是本地，master是远程分支

#### 代码回滚

在上传代码到远程仓库的时候，不免会出现问题，任何过程都有可能要回滚代码：

##### 1、在工作区的代码（checkout ~ 修改工作区文件）

git checkout -- a.txt # 丢弃某个文件，或者  
git checkout -- . # 丢弃全部

注意：git checkout -- . 丢弃全部，也包括：新增的文件会被删除、删除的文件会恢复回来、修改的文件会回去。这几个前提都说的是，回到暂存区之前的样子。对之前保存在暂存区里的代码不会有任何影响。对commit提交到本地分支的代码就更没影响了。当然，如果你之前压根都没有暂存或commit，那就是回到你上次pull下来的样子了。

#### 2、代码 git add 到缓存区，并未 git commit 提交（reset ~ 修改暂存区文件）

git reset HEAD .  
git reset HEAD a.txt

注意：这个命令仅改变暂存区，并不改变工作区，这意味着在无任何其他操作的情况下，  
工作区中的实际文件同该命令运行之前 无任何变化

#### 3、代码 git commit 到本地分支，但没有 git push 到远程 （git reset --hard ~ commit 之后）

git log # 得到你需要回退一次提交的commit id  
git reset --hard <commit_id> # 回到其中你想要的某个版本  
git reset --hard HEAD^ # 回到最新的一次提交  
git reset HEAD^ # 此时代码保留，回到 git add 之前

#### 4、代码 git push 把修改提交到远程仓库 (git reset || git revert)

-   （1）通过git reset是直接删除指定的commit

git log # 得到你需要回退一次提交的commit id  
git reset --hard <commit_id>  
git push origin HEAD --force # 强制提交一次，之前错误的提交就从远程仓库删除

-   (2) 通过git revert是用一次新的commit来回滚之前的commit

git log # 得到你需要回退一次提交的commit id  
git revert <commit_id> # 撤销指定的版本，撤销也会作为一次提交进行保存

-   （3） git revert 和 git reset的区别  
    (a). git revert是用一次新的commit来回滚之前的commit，此次提交之前的commit都会被保留( 会有 两次 commit id)；  
    (b). git reset是回到某次提交，提交及之前的commit都会被保留，但是此commit id之后的修改都会被删除 ( 只有一次 commit id)

  
  
作者：_画圆_  
链接：https://www.jianshu.com/p/c55958563f5a  
来源：简书  
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
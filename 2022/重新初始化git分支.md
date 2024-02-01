

很多人有一个大胆的想法，如何清空某个分支里面所有的 commit 呢？还有一个场景，不熟悉 [[Git]] 的程序员门新建分支基于某个分支创建的，但是可能这个分支的历史 Commits 是不需要的。那么，下面我就说一下如何将分支的历史 Commits 清空吧！

新建一个空白分支

首先，你应该切换到你需要清空的分支，然后执行 (我们拟定为 test 吧)：

git checkout --orphan null_branch

然后你会发现，你分支下的所有文件都成了待添加状态，我们可以直接执行 git add -A 添加，然后先存在 null_branch 中

git commit -am "Init commit."

删除旧本地分支

git branch -D test

删除是为了将 null_branch 重命名为之前的分支名称

然后执行重命名为之前的分支名称：

git branch -m test

进行强制提交到远程仓库

git push -f origin test

可能遇到的问题

如果你使用过 GitHub 或者 SourceTree 可能会遇到：

This repository is configured for Git LFS but ‘git-lfs’ was not found on your path. If you no longer wish to use Git LFS, remove this hook by deleting .git/hooks/pre-push.

这样的错误，很简单，执行

rm -rf .git/hooks/pre-push 

删除这个 hook 即可。
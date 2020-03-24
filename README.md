# git
## git 本地仓库

### git 从远程仓库 clone 到本地

git clone https://github.com/marquis11/git.git



### git 添加修改文件

git add  &lt;file&gt;

git add . ：他会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

git add -u ：他仅监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写）

git add -a ：是上面两个功能的合集（git add --all的缩写）



### git 提交至本地仓库

git commit - m ""     -m 为描述性信息



### git 日志操作

git log  可以查看最终的操作  ， q 为退出日志;

git reflog   用来记录你的每一次命令;



### git 版本回退

git reset --hard HEAD^

git reset --hard 1094a  -- 为 commit id  不必全写，历史的id 可以通过  git reflog  查询；

现在总结一下：

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。

git reset --soft HEAD^   版本回退，但是不会去除修改部分。



### git 撤销修改

1, git checkout -- file 可以丢弃工作区的修改；

2, git add 暂存区,  没有 commit ，用命令`git reset HEAD `&lt;file&gt;可以把暂存区的修改撤销掉（unstage），重新放回工作区；



### git 删除文件

1, 正常删除  git rm &lt;file&gt;  ，并且 要提交  git commit - m "...."

2，错误删除  ，在已经 commit  的情况下  通过 git checkout -- &lt;file&gt; 可以恢复误删的文件，

注意： 

1，从来没有被添加到版本库就被删除的文件，是无法恢复的！

2，命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。



## git 远程仓库

### 添加到远程仓库 （github）

1， 判断是否存在 ssh key

​     1.1    文件夹查看 C:/Users/user/.ssh/id_rsa.pub 

​     1.2     git bash方式查看  $ ls -al ~/.ssh

2,  若没有 通过  $ ssh-keygen -t rsa -C "377@Gmail.com"  ，然后一路回车 （默认选项）即可。id_rsa.pub是公钥，里面就是SSH key。

3，第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的SSH key 内容；

4， git push -u origin master   -u 是第一次提交至远程仓库要带的



## 分支管理

### 创建分支

1，首先，我们创建`dev`分支，然后切换到`dev`分支：

$ git checkout -b dev
Switched to a new branch 'dev'

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

$ git branch dev
$ git checkout dev
Switched to branch 'dev'

git branch 为 查看当前分支



2，合并分支

 git merge dev

git merge 命令用于合并指定分支到当前分支。

3，删除分支

git branch -d dev 



小结

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch `

切换分支：`git checkout `

创建+切换分支：`git checkout -b `

合并某分支到当前分支：`git merge `

删除分支：`git branch -d `



### 合并分支

1，分支 commit 后；

2，主分支 commit 后 ；

3 ，主分支 需要  git merge  feature1  分支 时候显示冲突，需要手动合并，

在手动合并后  ，在 git add file ,  git  commit ；



### 通过 --no-ff 合并

1，git merge --no-ff -m "merge with no-ff"  &lt;branchName&gt;  ；

2，-m 是再主分支上进行的提交；

3，git log --graph --pretty=oneline --abbrev-commit 查询log 会显示合并记录；



## Bug 分支管理



软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交：

```
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt
```

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```
$ git stash
Saved working directory and index state WIP on dev: f52c633 add merge
```

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git checkout -b issue-101
Switched to a new branch 'issue-101'
```

现在修复bug，需要把“Git is free software ...”改为“Git is a free software ...”，然后提交：

```
$ git add readme.txt 
$ git commit -m "fix bug 101"
[issue-101 4c805e2] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

修复完成后，切换到`master`分支，并完成合并，最后删除`issue-101`分支：

```
$ git switch master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 6 commits.
  (use "git push" to publish your local commits)

$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 readme.txt | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

太棒了，原计划两个小时的bug修复只花了5分钟！现在，是时候接着回到`dev`分支干活了！

```
$ git switch dev
Switched to branch 'dev'

$ git status
On branch dev
nothing to commit, working tree clean
```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```
$ git stash list
stash@{0}: WIP on dev: f52c633 add merge
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

```
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用`git stash list`查看，就看不到任何stash内容了：

```
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```
$ git stash apply stash@{0}
```


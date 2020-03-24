# git
## git 本地仓库

### git 从远程仓库 clone 到本地

git clone https://github.com/marquis11/git.git



### git 添加修改文件

git add  <file>

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



### git 撤销修改

1, git checkout -- file 可以丢弃工作区的修改；

2, git add 暂存区,  没有 commit ，用命令`git reset HEAD `<file>可以把暂存区的修改撤销掉（unstage），重新放回工作区；



### git 删除文件

1, 正常删除  git rm <file>  ，并且 要提交  git commit - m "...."

2，错误删除  ，在已经 commit  的情况下  通过 git checkout -- <file> 可以恢复误删的文件，

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


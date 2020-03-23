# git
git 详解

### git 从远程仓库 clone 到本地

git clone https://github.com/marquis11/git.git



### git 添加修改文件

git add  <file>

git add . ：他会监控工作区的状态树，使用它会把工作时的所有变化提交到暂存区，包括文件内容修改(modified)以及新文件(new)，但不包括被删除的文件。

git add -u ：他仅监控已经被add的文件（即tracked file），他会将被修改的文件提交到暂存区。add -u 不会提交新文件（untracked file）。（git add --update的缩写）

git add -A ：是上面两个功能的合集（git add --all的缩写）



### git 提交至本地仓库

git commit - m ""     -m 为描述性信息


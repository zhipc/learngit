一：创建与合并分支
1、创建dev分支，然后切换到dev分支：
$ git checkout -b dev
git checkout命令加上-b参数表示创建并切换，相当于以下两条命令：
$ git branch dev
$ git checkout dev
2、用git branch命令查看当前分支：
$ git branch
3、切换回master分支：
$ git checkout master
4、把dev分支的工作成果合并到master分支上：
$ git merge dev (git merge命令用于合并指定分支到当前分支)
5、删除dev分支：
$ git branch -d dev

二：解决冲突
1、当master分支和feature1分支各自都分别有新的提交Git无法执行“快速合并”，只能试图把各自的修改合并起来，
但这种合并就可能会有冲突，我们试试看：
$ git merge feature1
果然冲突了！Git告诉我们，readme.txt文件存在冲突，必须手动解决冲突后再提交。git status也可以告诉我们冲突的文件：
$ git status
直接查看readme.txt的内容：
$ cat readme.txt
修改冲突后再提交
2、用带参数的git log也可以看到分支的合并情况：
$ git log --graph --pretty=oneline --abbrev-commit

三：分支管理策略
合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
$ git merge --no-ff -m "merge with no-ff" dev
Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

四：Bug分支
1、Git还提供了一个stash功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：
$ git stash
2、用git stash list命令查看“储藏”的工作现场：
$ git stash list
3、恢复工作现场，有两个办法：
一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
$ git stash apply stash@{0}  （恢复指定的stash：$ git stash apply stash@{0}）
另一种方式是用git stash pop，恢复的同时把stash内容也删了
$ git stash pop

五：Feature分支
开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过git branch -D <name>强行删除。
$ git branch -D feature-vulcan

六：多人协作
1、查看远程库的信息，用git remote：
$ git remote
或者，用git remote -v显示更详细的信息：
$ git remote -v
2、推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：
$ git push origin master
3、创建远程origin的dev分支到本地，可以使用这个命令创建本地dev分支：
$ git checkout -b dev origin/dev
4、推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：
$ git pull
5、git pull也失败了，原因是没有指定本地dev分支与远程origin/dev分支的链接，根据提示，设置dev和origin/dev的链接：
$ git branch --set-upstream dev origin/dev
6、这回git pull成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的解决冲突完全一样。解决后，提交，再push

多人协作的工作模式通常是这样：
首先，可以试图用git push origin branch-name推送自己的修改；
如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用git push origin branch-name推送就能成功！
如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令git branch --set-upstream branch-name origin/branch-name。


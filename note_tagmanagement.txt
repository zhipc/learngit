一：创建标签
1、切换到需要打标签的分支上：
$ git branch
$ git checkout master
2、敲命令git tag <name>就可以打一个新标签：
$ git tag v1.0
3、可以用命令git tag查看所有标签：
$ git tag
4、默认标签是打在最新提交的commit上的。如果忘了打标签，那就找到历史提交的commit id，然后打上就可以了：
$ git log --pretty=oneline --abbrev-commit
如果要对commit id是6224937的历史版本打标签：
$ git tag v0.9 6224937
5、可以用git show <tagname>查看标签信息：
$ git show v0.9
创建带有说明的标签，用-a指定标签名，-m指定说明文字：
$ git tag -a v0.1 -m "version 0.1 released" 3628164

二：操作标签
1、删除标签：
$ git tag -d v0.1
2、推送某个标签到远程，使用命令git push origin <tagname>：
$ git push origin v1.0
一次性推送全部尚未推送到远程的本地标签：
$ git push origin --tags
3、如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除：
$ git tag -d v0.9
然后，从远程删除。删除命令也是push，但是格式如下：
$ git push origin :refs/tags/v0.9


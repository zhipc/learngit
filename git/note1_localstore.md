分布式版本控制系统通常也有一台充当“中央服务器”的电脑，但这个服务器的作用仅仅是用来方便“交换”大家的修改，没有它大家也一样干活，只
是交换修改不方便而已。

#### 一：安装
在Windows上安装Git
msysgit是Windows版的Git，从[这里](http://msysgit.github.io/)下载，然后按默认选项安装即可。
安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

#### 二：设置
安装完成后，还需要最后一步设置，在命令行输入：
$ git config --global user.name "Your Name"
$ git config --global user.email "email@example.com"
注意git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户
名和Email地址。

#### 三：创建版本库
* 创建一个空目录：
$ mkdir learngit
$ cd learngit
$ pwd
/Users/michael/learngit
* 这样系统就不会去进行换行符的转换了
$ git config --global core.autocrlf false  
* 通过git init命令把这个目录变成Git可以管理的仓库：
$ git init
* 编写一个readme.txt文件，内容如下：
Git is a version control system.
Git is free software.
* 用命令git add告诉Git，把文件添加到仓库：
$ git add readme.txt（实际上就是把文件修改添加到暂存区）
* 用命令git commit告诉Git，把文件提交到仓库：
$ git commit -m "wrote a readme file"（实际上就是把暂存区的所有内容提交到当前分支）

#### 四：提交前的状态查看和版本回退
* git status命令可以让我们时刻掌握仓库当前的状态
$ git status
* 用git diff这个命令看看具体修改了什么内容：
$ git diff readme.txt 
* 用git log命令查看历史记：
$ git log（或者$ git log --pretty=oneline）
* 把当前版本回退到上一个版本，就可以使用git reset命令：
$ git reset --hard HEAD~2（HEAD~1代表回退到上上个版本）
指定回到未来的某个版本：
$ git reset --hard 3628164
* 命令git reflog用来记录你的每一次命令：
$ git reflog

#### 五：撤销修改和删除文件
* 场景一：git checkout -- file可以丢弃工作区的修改：
$ git checkout -- readme.txt
命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。
* 场景二：用命令git reset HEAD file可以把暂存区的修改撤销掉（unstage），重新放回工作区：
$ git reset HEAD readme.txt
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区
* 场景三：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。
* 删除没用的文件：
    - 在文件管理器中把没用的文件删了，或者用rm命令删了：
$ rm test.txt
    - git status命令会立刻告诉你哪些文件被删除了：
$ git status
    - 现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：
$ git rm test.txt
另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：
$ git checkout -- test.txt
git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。






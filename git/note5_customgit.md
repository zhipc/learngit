>让Git显示颜色，会让命令输出看起来更醒目：
>$ git config --global color.ui true
>我们在下面还会介绍如何更好地配置Git，以便让你的工作更高效。

##### 一：忽略特殊文件
* 忽略某些文件时，需要编写.gitignore；.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！
* 忽略文件的原则是：
   - 忽略操作系统自动生成的文件，比如缩略图等；
   - 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的.class文件；
   - 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件。
* 检验.gitignore的标准是git status命令是不是说working directory clean。

#### 二：配置别名
* 配置$ git log 为 git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
* 每个仓库的Git配置文件都放在.git/config文件中：
$ cat .git/config 
* 当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中：
$ cat ~/.gitconfig
* 查看别名
$ git config --get-regexp alias(或者git config --list | grep alias)
* 取消别名 
$ git config --global --unset alias.st

#### 三：搭建git服务器
* 搭建步骤：
   - 第一步，安装git：
$ sudo apt-get install git
   - 第二步，创建一个git用户，用来运行git服务：
$ sudo adduser git
   - 第三步，创建证书登录：
收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
   - 第四步，初始化Git仓库：
先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：
$ sudo git init --bare sample.git
Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：
$ sudo chown -R git:git sample.git
   - 第五步，禁用shell登录：
出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：
git:x:1001:1001:,,,:/home/git:/bin/bash
改为：
git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。
   - 第六步，克隆远程仓库：
现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：
$ git clone git@server:/srv/sample.git

* 管理公钥
如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用Gitosis来管理公钥。
* 管理权限
有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。Gitolite就是这个工具。

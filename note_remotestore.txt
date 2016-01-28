注册一个GitHub账号，就可以免费获得Git远程仓库

一：使得远程仓库可以被访问：
由于你的本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以，需要一点设置：
第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，
可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：
$ ssh-keygen -t rsa -C "youremail@example.com"
你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，
不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容

二：添加远程库
现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，
GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。
1、创建一个新的空仓库
2、在本地的learngit仓库下运行命令：
$ git remote add origin git@github.com:michaelliao/learngit.git
3、把本地库的所有内容推送到远程库上：
$ git push -u origin master
4、从现在起，只要本地作了提交，就可以通过命令：
$ git push origin master
把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！


三：从远程库克隆：
1、创建一个新的仓库：
2、用命令git clone克隆一个本地库：
$ git clone git@github.com:michaelliao/gitskills.git

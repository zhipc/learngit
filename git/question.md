#### git clone
git clone 时用git clone git@github.com:zhipc/codeStorage.git，而不是git clone git://github.com:zhipc/codeStorage.git，否则不能push

#### file rename
git add -A
它能stages所有文件，而之前用的
git add .
只能stages新文件和被修改文件，没有被删除文件

#### could not resolve hostname
* step1. ping github.com192.30.252.128 github.com
获取到github.com的ip为192.30.252.128
* step2. 在/etc/hosts中添加一行如下:
192.30.252.128 github.com

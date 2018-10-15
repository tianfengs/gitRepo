git服务器地址：

http://10.168.1.76/users/sign_in


1. 注册username和email
2. 在自己的电脑上安装git客户端，下载地址：
	https://git-scm.com/
3. 配置git环境并连接
   a.在本地电脑中注册username和email地址：
	git config --global user.name "zhangsan"
	git config --global user.email "zs@gmail.com"
   b.在本地电脑建立ssh秘钥，并把公钥（C:\Users\krk10\.ssh\id_rsa.pub）的内容粘贴到git远程服务器：
	ssh-keygen -t RSA -C "user@gmail.com"
   b.学习在本地克隆远程库的建库方法
4. 学习在命令行中使用git，进行版本更替，建立和使用分支技巧，学习使用在线浏览器页面使用git的方法。



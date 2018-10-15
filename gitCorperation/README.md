README 
===========================

****

## 目录

* [软件改动流程](#)
	* [主线流程](#主线流程)	
	* [分支流程一](#分支流程一没有代码审核模式)	
	* [分支流程二](#分支流程二项目master审核模式)	
	* [项目master操作](#项目master操作)
	* [其它规则](#其它规则)
* [Git 常用命令](#Git 常用命令)

* [Git 合并分支](#Git 合并分支)
	* 合并本地分支
	
	* 合并到远程分支

* [Git 常见问题](#Git常见问题)
	* error: pathspec 'master' did not match any file(s) known to git.

	* push error: failed to push some refs to 'git@.../test.git'
 
* [push到远程分支 目前 建议方案](#push到远程分支目前建议方案)
* [Git 官方文档
](#)
****


### 内容

------

## 软件改动流程

### <a id="#1">主线流程</a>


>1. 首先要先在版本服务器上的issues里建立标签，描述问题，并添加相应label，如果处于某个milestone里，要标记milestone名称

>2. 在issues里对对应问题进行讨论通过


>3. 把本地dev分支更新到最新


>4. 在分支流程二中：在本地创建开发者分支，如：tianfeng，huangyf分支


>5. 建立对应issue分支，分支名称为issue+No.


>6. 编码


>7. 本地测试通过，创建提交，并绑定对应issue


>8. 如果commit多次，需要用rebase -i 合并成一次提交



#### 分支流程一：（没有代码审核模式）

>1. 更新本地dev分支为最新


>2. 把issueXXX分支合并到本地dev分支，如有冲突，解决冲突，提交


>3. 把本地dev分支推送到远程dev


#### 分支流程二：（项目master审核模式）

>1. 把issueXXX分支合并到本地开发者名字分支


>2. 推送开发者名字分支到服务器

>
3. 在服务器发起merge request，把开发者分支合并到dev分支 


#### 项目master操作

>1. 项目master收到请求


>2. fetch dev分支和申请的开发者分支


>3. git log dev..origin/开发者名字 查看提交内容，或git log -p查看区别 


>4. git merge origin/开发者名字分支 到本地dev分支


>5. 审核代码，解决冲突


>6. 如果审核未通过，reset回滚，或者取消合并


>7. 审核通过，解决完冲突，push到远程dev分支




### 其它规则：
本地不需要推送到远程的分支，每次commit，都写个单行-m注释就可以，合并的时候用no-ff，再写一个比较详细的合并说明就可以了

===========================

****


### <a id="#2">Git 常用命令</a>
* 最常用（使用频率最高）	

	创建版本库：git init


	查看分支列表 ：git branch  （不带参数表示查看本地分支，-r 表示查看远程分支；-a表示查看所有分支）


	创建分支：git branch 分支名


	切换分支：git checkout 分支名


	创建并切换分支：git checkout -b 分支名


	添加文件到git ： git add 文件名 （.表示添加所有文件到git）


	提交 ：git commit -m"备注说明"


	查看git当前状态： git status


	克隆远程库：git clone git@10.168.1.76:nanfei9330/xx.git0


	本地与服务器端同步：git pull  (相当于 fetch 跟 merge)（不建议使用）


	获取远程最新版本：git fetch  常用命令为 git fetch origin 远程分支名x:本地分支名x （获取远程某分支到本地分支：本地分支没有时会自动创建）


	对比分支之间的差异：git diff 分支名 （表示对比某分支跟当前分支在文件上的差异）


	利用图形界面解决冲突： git difftool / git mergetool
	
	合并分支：git merge 分支名
	
	推送到远程分支：git push origin 本地分支名：远程分支名

* 较常用（使用频率较高）


	查看已被提交：git ls-files


	删除一个文件：git rm 文件名


	删除分支 ：git branch -D 分支名


	查看commit日志：git log


	查看尚未暂存的更新（未执行add命令）：git diff

	撤销最近一次commit：git reset HEAD^

 
	
* 其他命令

	
	[Git 常用命令大全](https://blog.csdn.net/halaoda/article/details/78661334)


### <a id="#3">Git 合并分支</a>

* 合并本地分支

	![本地合并分支](http://10.168.1.76/root/FileProject/uploads/273b101198512dd135e02ce9c0e7bdb0/%E6%9C%AC%E5%9C%B0%E5%90%88%E5%B9%B6%E5%88%86%E6%94%AF.png "本地合并") 

* 合并到远程分支


	1、通过 git fetch origin dev:temp1 (获取远程dev 分支上最新的版本信息，到本地的 temp1分支)
	
	2、创建 temp 分支：git checkout -b temp
	
	3、通过push命令提交到远程分支

	![合并到远程分支1](http://10.168.1.76/root/FileProject/uploads/a8b54d39dab78c55403b7a9983f1d4ec/%E8%BF%9C%E7%A8%8B1.png "合并到远程分支1")  
	![合并到远程分支1](http://10.168.1.76/root/FileProject/uploads/8a99d04dea9ada4566437d6d3501cc7a/%E8%BF%9C%E7%A8%8B2.png "合并到远程分支1")  

## <a id="#4">Git 常见问题</a>
* error: pathspec 'master' did not match any file(s) known to git.

	问题说明：在空的git版本库中切换到master失败（在空的版本库中使用branch 命令创建分支也会失败）
	问题产生原因：1、本地版本库没有跟远程库进行关联。2、本地版本库没有任何文件。
	解决方案：1、关联远程库获取所有分支。2、本地版本库新建文件以及分支。
* push error: failed to push some refs to 'git@.../test.git'
	问题说明：本地分支推送到远程分支时出现错误。
	问题产生原因：本地分支跟远程分支版本不一致。
	解决方案：[合并到远程分支](#合并到远程分支)	

## <a id="#5">push到远程分支 目前 建议方案</a>
	方案具体操作说明：
	1、本地添加、修改好功能或者文件后（add、commit），先在本地版本库进行merge（合并）到某个分支，存在冲突时先解决本地的冲突问题。
	2、获取远程分支最新的版本。比如要合并到的远程分支为dev分支，则通过 git fetch origin dev:temp 获取dev最新的版本到temp分支。
	3、通过 git diff temp 查看temp分支与当前分支的不同。
	4、存在冲突时，可以通过 git difftool 解决分支（图形界面解决分支）
	5、冲突解决后，进行add、commit。
	6、通过 git push origin 本地分支名：dev 命令推送到远程分支
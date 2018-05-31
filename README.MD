# Git 一些资料

[Release 分支](#aaa)

# 阅读 A successful Git branching model笔记

摘自[https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/ "A successful Git branching model")

核心的版本库包括两个主要分支

* master
* develop

其它分支包括

* Feature 分支
* Release 分支
* Hotfix 分支

### Feature分支

分离自develop分支。或者叫topic分支。

是用来开发一个功能，以后可以发布的分支。

它最终开发完毕，要被合并进develop分支。

它只存在于开发者的本地repo，不推送到远程origin。

在develop下开一个新的myfeature分支：

```
$ git checkout -b myfeature develop
```

开发完，合并到develop分支

```
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff myfeature
Updating ea1b82a..05e9557
(Summary of changes)
$ git branch -d myfeature
Deleted branch myfeature (was 05e9557).
$ git push origin develop
```

--no-ff可以使合并时，总是建立一个commit节点。

### Release分支

```
<div id="aaa">sdfdsf</div> 
```

分离自develop

是用来准备进行产品发布前的分支。可以进行小bug修复和准备元数据，如版本号，产品出厂日期等等。

开发完毕，要被合并进develop和master

命名规则：release-\*

在develop下开一个新的发布分支

```
$ git checkout -b release-1.2 develop
Switched to a new branch "release-1.2"
$ ./bump-version.sh 1.2
Files modified successfully, version bumped to 1.2.
$ git commit -a -m "Bumped version number to 1.2"
[release-1.2 74d9424] Bumped version number to 1.2
1 files changed, 1 insertions(+), 1 deletions(-)
```

./bump-version.sh是一个虚构的改变产品版本号的脚本。  
当一切就绪，准备把这个分支真正发布的时候：

1. 合并到master分支
2. 更新到master上时，必需打上tag，以方便将来对这个版本进行回溯
3. release分支也要合并回develop分支，将来进行版本回溯时，也包括release分支中的bug修复

前两步代码：

```
$ git checkout master
Switched to branch 'master'
$ git merge --no-ff release-1.2
Merge made by recursive.
(Summary of changes)
$ git tag -a 1.2
```

第三步：

```
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff release-1.2
Merge made by recursive.
(Summary of changes)
```

这一步可能会导致冲突，解决冲突。

### Hotfix分支（补丁分支）

分离自master分支

开发完毕，要被合并回master和develop分支

命名规则：hotfix-_（或 bugfix-_）

在master分支下开一个新的补丁分支

```
$ git checkout -b hotfix-1.2.1 master
Switched to a new branch "hotfix-1.2.1"
$ ./bump-version.sh 1.2.1
Files modified successfully, version bumped to 1.2.1.
$ git commit -a -m "Bumped version number to 1.2.1"
[hotfix-1.2.1 41e61bb] Bumped version number to 1.2.1
1 files changed, 1 insertions(+), 1 deletions(-)
```

开发后，不要忘记修改软件版本号之后再更新（commit）

```
$ git commit -m "Fixed severe production problem"
[hotfix-1.2.1 abbe5d6] Fixed severe production problem
5 files changed, 32 insertions(+), 17 deletions(-)
```

开发完合并回master和develop分支的方法，和release分支相同：

1.合并回master

```
$ git checkout master
Switched to branch 'master'
$ git merge --no-ff hotfix-1.2.1
Merge made by recursive.
(Summary of changes)
$ git tag -a 1.2.1
```

2.合并回develop

```
$ git checkout develop
Switched to branch 'develop'
$ git merge --no-ff hotfix-1.2.1
Merge made by recursive.
(Summary of changes)
```

3.删除hotfix分支

```
$ git branch -d hotfix-1.2.1
Deleted branch hotfix-1.2.1 (was abbe5d6).
```

### 总结

![](http://10.168.1.76/help/workflow/gitdashflow.png)

---

# 软件改动流程

## 主线流程

> 1. 首先要先在版本服务器上的issues里建立标签，描述问题，并添加相应label，如果处于某个milestone里，要标记milestone名称
> 2. 在issues里对对应问题进行讨论通过
> 3. 把本地dev分支更新到最新
> 4. 在分支流程二中：在本地创建开发者分支，如：tianfeng，huangyf分支
> 5. 建立对应issue分支，分支名称为issue+No.
> 6. 编码
> 7. 本地测试通过，创建提交，并绑定对应issue
> 8. 如果commit多次，需要用rebase -i 合并成一次提交

### 分支流程一：（没有代码审核模式）

> 1. 更新本地dev分支为最新
> 2. 把issueXXX分支合并到本地dev分支，如有冲突，解决冲突，提交
> 3. 把本地dev分支推送到远程dev

### 分支流程二：（项目master审核模式）

> 1. 把issueXXX分支合并到本地开发者名字分支
> 2. 推送开发者名字分支到服务器\`\`
> 3. 在服务器发起merge request，把开发者分支合并到dev分支

### 项目master操作

> 1. 项目master收到请求
> 2. fetch dev分支和申请的开发者分支
> 3. git log dev..origin/开发者名字 查看提交内容，或git log -p查看区别 
> 4. git merge origin/开发者名字分支 到本地dev分支
> 5. 审核代码，解决冲突
> 6. 如果审核未通过，reset回滚，或者取消合并
> 7. 审核通过，解决完冲突，push到远程dev分支

---

## 其它规则

本地不需要推送到远程的分支，每次commit，都写个单行-m注释就可以，合并的时候用no-ff，再写一个比较详细的合并说明就可以了

---

# fetch更新本地仓库两种方式：

> 方法一

```
$ git fetch origin master             //从远程的origin仓库的master分支下载代码到本地的origin master
$ git log -p master.. origin/master   //比较本地的仓库和远程参考的区别
$ git merge origin/master             //把远程下载下来的代码合并到本地仓库，远程的和本地的合并
```

> 方法二

```
$ git fetch origin master:temp //从远程的origin仓库的master分支下载到本地并新建一个分支temp
$ git diff temp                //比较master分支和temp分支的不同
$ git merge temp               //合并temp分支到master分支
$ git branch -d temp           //删除temp
```

---

# 本地Check out, review, and merge

> 本内容引用自：  
> [http://10.168.1.76/help/user/project/merge\_requests/index.md\#tips](http://10.168.1.76/help/user/project/merge_requests/index.md#tips)

步骤 1. 为merge request进行Fetch和创建本地分支

```
git fetch origin
git checkout -b issue8 origin/issue8
```

Step 2. Review the changes locally，代码审核修改

Step 3. Merge the branch and fix any conflicts that come up，合并到dev分支，解决冲突

```
git checkout dev
git merge --no-ff issue8
```

Step 4. Push the result of the merge to GitLab，推送dev分支到远程GitLab服务器

```
git push origin dev
```

Tip: You can also checkout merge requests locally by following these guidelines.

## Checkout merge requests locally，本地切换到merge requests

merge requests 包括所有的版本历史，并且另外包含一个跟merge request相关的commit提交。这  
里有一个本地切换merge requests的技巧。

注意：即使远程project是一个fork的，甚至是私有的fork，也可以进行本地操作。

## Checkout locally by adding a git alias，通过增加一个git别名本地化

在你的文件 ~/.gitconfig 里增加如下代码:

```
[alias]
  mr = !sh -c 'git fetch $1 merge-requests/$2/head:mr-$1-$2 && git checkout mr-$1-$2' -
```

Now you can check out a particular merge request from any repository and any  
remote.   
For example, to check out the merge request with ID 5 as shown in GitLab  
from the upstream remote, do:  
例如：从远程仓库检出ID为5的merge request：

```
git mr origin 5
```

This will fetch the merge request into a local mr-origin-5 branch and check  
it out.  
这将fetch这个merge request到本地的mr-upstream-5分支，并切换到这个分支。

## Checkout locally by modifying .git/config for a given repository

Locate the section for your GitLab remote in the .git/config file. It looks  
like this:

```
[remote "origin"]
    url = https://gitlab.com/gitlab-org/gitlab-ce.git
    fetch = +refs/heads/*:refs/remotes/origin/*
```

You can open the file with:

```
git config -e
```

Now add the following line to the above section:

```
fetch = +refs/merge-requests/*/head:refs/remotes/origin/merge-requests/*
```

In the end, it should look like this:

```
[remote "origin"]
    url = https://gitlab.com/gitlab-org/gitlab-ce.git
    fetch = +refs/heads/*:refs/remotes/origin/*
    fetch = +refs/merge-requests/*/head:refs/remotes/origin/merge-requests/*
```

Now you can fetch all the merge requests:

```
git fetch origin

...
From https://gitlab.com/gitlab-org/gitlab-ce.git
 * [new ref]         refs/merge-requests/1/head -> origin/merge-requests/1
 * [new ref]         refs/merge-requests/2/head -> origin/merge-requests/2
...
```

And to check out a particular merge request:

```
git checkout origin/merge-requests/1
```




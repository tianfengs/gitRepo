\#

Git 一些资料



\1. [A successful Git branching model笔记](#1)

\* 1.1. [Feature 分支](#1.1)

\* 1.2. [Release 分支](#1.2)

\* 1.3. [Hotfix分支（补丁分支）](#1.3)

\* 1.4. [总结](#1.4)

\2. [软件改动流程](#2)

\* 2.1. [主线流程](#2.1)

\* 2.2. [分支流程一：（没有代码审核模式）](#2.2)

\* 2.3. [分支流程二：（项目master审核模式）](#2.3)

\* 2.4. [项目master操作](#2.4)

\* 2.5. [其它规则](#2.4)

\3. [fetch更新本地仓库两种方式](#3)

\* 3.1. [方法一](#3.1)

\* 3.2. [方法二](#3.2)

\4. [本地Check out, review, and merge](#4)

\* 4.1. [常规方法](#4.1)

\* 步骤 1. [为merge request进行Fetch和创建本地分支](#4.1a)

\* Step 2. [Review the changes locally，代码审核修改](#4.1b)

\* Step 3. [Merge the branch and fix any conflicts that come up，合并到dev分支，解决冲突](#4.1c)

\* Step 4. [Push the result of the merge to GitLab，推送dev分支到远程GitLab服务器](#4.1d)

\* 4.2. [Checkout merge requests locally，本地切换到merge requests](#4.2)

\* 4.3. [Checkout locally by adding a git alias，通过增加一个git别名本地化](#4.3)

\* [[alias]](#4.3a)

\* [mr = !sh -c 'git fetch $1 merge-requests/$2/head:mr-$1-$2 && git checkout mr-$1-$2' -](#4.3b)

*

\* [git mr origin 5](#4.3c)

\* 4.4. [Checkout locally by modifying .git/config for a given repository](#4.4)

\5. [Github关于ignore文件的说明](#5)

\6. [使用命令创建github代码仓库，push本地仓库到github远程代码仓库](#6)

\7. [附录文件说明](#7)

\* 范例.ssh文件夹：多用户在同一台电脑，以及，登录多个远程服务器配置范例（科瑞康公司电脑配置）

\* ignore.png：第5节说明网页截图

\* git服务器说明.doc:服务器配置的一些经验

\* git远程删除分支后本地git branch -a 依然能看到的解决办法.MD：如题

\8. [本地创建远程仓库](#8)

\9. [Git远程仓库回退方法](GIT远程仓库回退方法.md)



1. ## A successful Git branching model笔记



\>摘自[https://nvie.com/posts/a-successful-git-branching-model/](https://nvie.com/posts/a-successful-git-branching-model/ "A successful Git branching model")



核心的版本库包括两个主要分支



\* master

\* develop



其它分支包括



\* Feature 分支

\* Release 分支

\* Hotfix 分支



\## <a name="1.1"/>1.1. Feature分支



分离自develop分支。或者叫topic分支。



是用来开发一个功能，以后可以发布的分支。



它最终开发完毕，要被合并进develop分支。

##<span name="2"/>2. 软件改动流程
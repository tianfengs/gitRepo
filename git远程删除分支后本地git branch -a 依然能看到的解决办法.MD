# git 删除远程分支

先查看远程分支

	git branch -r

使用下面两条命令删除远程分支

	git branch -r -d origin/branch-name  
	git push origin :branch-name  

--------------------------------
#git远程删除分支后，本地git branch -a 依然能看到的解决办法。

使用 git branch -a 命令可以查看所有本地分支和远程分支（git branch -r 可以只查看远程分支） 
发现很多在远程仓库已经删除的分支在本地依然可以看到。

	$ git branch -a
	
	  movtop
	  task_develop
	* weibo
	  remotes/origin/HEAD -> origin/task_develop
	  remotes/origin/develop
	  remotes/origin/fix_composer_repositories_type
	  remotes/origin/join_weixin_module
	  remotes/origin/master
	  remotes/origin/mining-backup
	  remotes/origin/movtop
	  remotes/origin/right
	  remotes/origin/schedule_dev
	  remotes/origin/stuff_web_fix
	  remotes/origin/task_develop
	  remotes/origin/task_idea
	  remotes/origin/task_temp
	  remotes/origin/task_yqj
	  remotes/origin/weibo
	  remotes/origin/weixin_temp


使用命令 git remote show origin，可以查看remote地址，远程分支，还有本地分支与之相对应关系等信息。

	$ git remote show origin  
	
	* remote origin
	  Fetch URL: https://xxx@gitlab.com/xxx/xxx.git
	  Push  URL: https://xxx@gitlab.com/xxx/xxx.git
	  HEAD branch: task_develop
	  Remote branches:
	    master                                             tracked
	    mining-backup                                      tracked
	    refs/remotes/origin/develop                        stale (use 'git remote prune' to remove)
	    refs/remotes/origin/fix_composer_repositories_type stale (use 'git remote prune' to remove)
	    refs/remotes/origin/join_weixin_module             stale (use 'git remote prune' to remove)
	    refs/remotes/origin/movtop                         stale (use 'git remote prune' to remove)
	    refs/remotes/origin/right                          stale (use 'git remote prune' to remove)
	    refs/remotes/origin/schedule_dev                   stale (use 'git remote prune' to remove)
	    refs/remotes/origin/stuff_web_fix                  stale (use 'git remote prune' to remove)
	    refs/remotes/origin/task_temp                      stale (use 'git remote prune' to remove)
	    refs/remotes/origin/weibo                          stale (use 'git remote prune' to remove)
	    task_develop                                       tracked
	    task_idea                                          tracked
	    task_yqj                                           tracked
	    weixin_temp                                        tracked
	  Local branches configured for 'git pull':
	    movtop       merges with remote movtop
	    task_develop merges with remote task_develop
	    weibo        merges with remote weibo
	  Local ref configured for 'git push':
	    task_develop pushes to task_develop (up to date)

此时我们可以看到那些远程仓库已经不存在的分支，根据提示，使用 git remote prune origin 命令：

	$ git remote prune origin
	
	Pruning origin
	URL: https://xxx@gitlab.com/xxx/xxx.git
	 * [pruned] origin/develop
	 * [pruned] origin/fix_composer_repositories_type
	 * [pruned] origin/join_weixin_module
	 * [pruned] origin/movtop
	 * [pruned] origin/right
	 * [pruned] origin/schedule_dev
	 * [pruned] origin/stuff_web_fix
	 * [pruned] origin/task_temp
	 * [pruned] origin/weibo

这样就删除了那些远程仓库不存在的分支。
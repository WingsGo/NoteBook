## 撤销与合并操作	
	git revert <commit id>	# 提交一次commit以撤销某次commit
	git cherry-pick <commit id>		# 单独取得某次commit内容合并到分支上
	git rebase --noto <base branch> <commit id> <branch>	# 将branch commit部分内容变基合并到base branch上
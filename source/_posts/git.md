---
title: git
date: 2019-04-15 10:30:59
tags:  git
---

### git命令行常见问题

	##### 	换行符LF与CRLF转换问题

```bash
git config --global core.autocrlf false		不转换换行符
git config --global core.safecrlf true		转换换行符
	
```

	##### 	查看提交记录

```bash
git log			查看master分支的提交记录
git log -p		查看当前分支的提交记录
```

	##### 	版本回退

```bash
git reset --hard commit_id  回到某次commit，但是之后的修改都会停留在暂存区
git revert commit_id 是生成一个新的提交来撤销某次提交，此次提交之前的commit都会被保留
```

	##### 	合并分支

```
git merge 分支名  
git merge 分支名 --no-ff  禁止快进式合并，生成一个新的提交
两者的区别在于后者生成一个新的提交，仓库的路线图开叉
```


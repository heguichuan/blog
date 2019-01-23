---
title: git笔记
date: 2019/1/9 11:09:25
---

# git笔记

## 入门

> git 是xxxx

**示意图:**

## 基本命令

```bash
git status #查看当前状态
git pull #拉取远程仓库并与工作空间合并
git branch -a #查看分支情况
git checkout -b new_local_branch origin/some_branch #从远程分支检出新分支
git checkout some_branch #切换分支
git add [file]# 追踪修改
git commit [-am 'message'] #提交修改
git push [origin HEAD:origin_branch] #推送
git remote prune origin [--dry-run]# 清理[查看]远程已经被删除的冗余分支
```


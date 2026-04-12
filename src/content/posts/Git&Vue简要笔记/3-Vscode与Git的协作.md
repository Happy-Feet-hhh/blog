---
title: Vscode与Git的协作
published: 2026-03-30
description: "在VSCode中使用Git进行版本控制与GitHub协作"
tags: ["Git", "VSCode", "GitHub"]
category: Git&Vue
draft: false
---

# 基本操作
在了解了项目目录的功能之后，我们删除vite自带的样板代码，然后就可以看到vscode左侧的git页有十多个变动：
![](Pasted%20image%2020260330214138.png)
![](Pasted%20image%2020260330214234.png)
点击修改的文件还能查看diff：
![](Pasted%20image%2020260330214253.png)
在提交变化到仓库之前，我们需要先运行项目，检查是否有问题，输入：
```bash
npm run dev
```
然后在网页上，点击F12，进入开发者工具，然后点击控制台查看是否有报错：
![](Pasted%20image%2020260330214451.png)
没有报错，我们可以顺利提交，先将所有的更改添加到暂存区，使用`git add -A`，然后发现vscode的git页也有相应的更新:
![](Pasted%20image%2020260330214711.png)
然后我们将暂存区的文件和更改提交`commit`到本地仓库，当然，这里我新建了一个分支`01-清除样板代码`：
```bash
git checkout -b "01-清除样板代码"
```
![](Pasted%20image%2020260330214955.png)
`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：
```bash
$ git branch "01-清除样板代码"
$ git checkout "01-清除样板代码"
Switched to branch 01-清除样板代码'
```
然后，用`git branch`命令查看当前分支：
![](Pasted%20image%2020260330215214.png)
发现在我们新建的分支中。`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。在这个分支，我们提交一下代码：
![](Pasted%20image%2020260330215354.png)
然后我们将当前分支的代码推送到远程仓库：
```bash
git push origin 01-清除样板代码
```
![](Pasted%20image%2020260330215509.png)
根据过程可以发现，push推送创建了GitHub仓库上的一个PR：
![](Pasted%20image%2020260330215603.png)
同时，在GitHub仓库上，也会更新：
![](Pasted%20image%2020260330215622.png)
![](Pasted%20image%2020260330215631.png)
在GitHub页面上，我们可以由此创建PR：

![](Pasted%20image%2020260330215910.png)
合并后我们的main分支也会更新：
![](Pasted%20image%2020260330220001.png)
![](Pasted%20image%2020260330220044.png)

# 问题：git远程仓库更新了，合并了一个PR，但是本地的main分支没有更新怎么办？
在GitHub仓库上合并PR之后，发现，本地的main分支仍然没有变化：
![](Pasted%20image%2020260330220351.png)
![](Pasted%20image%2020260330220356.png)
这些样板代码还在。这个问题是因为 Git 是分布式版本控制系统，远程仓库合并 PR 后，只是**远程的`main`分支更新了**，你的本地仓库不会自动同步这个变更，需要手动执行拉取操作，把远程的更新同步到本地。

### 情况 1：本地 main 没有任何修改（最常见的情况）

如果你本地的`main`分支没做过任何修改、也没有未提交的内容，直接执行这两步就能完成同步：
```bash
# 1. 切换到本地的main分支 
git checkout main 
# 2. 拉取远程main分支的最新更新 
git pull
```
### 情况 2：本地 main 有修改，但我不需要了，想彻底和远程保持一致

如果你之前在本地`main`改了内容，现在不想要这些本地修改了，想让本地`main`完全和远程仓库一模一样，执行：
```bash
git checkout main # 先拉取远程的最新版本信息 
git fetch origin # 重置本地main，和远程的origin/main完全对齐 
# ⚠️ 警告：这个操作会彻底删除你本地main的所有未提交/未推送的修改，执行前一定要确认你不需要这些内容了！ 
git reset --hard origin/main
```

### 情况 3：本地 main 有修改，我想保留这些修改，同时合并远程更新

如果你本地`main`有自己的工作修改，不想丢掉，同时要合并远程的更新：
```bash
git checkout main  # 切换回main分支
# 先把你本地的修改临时暂存起来 
git stash # 拉取远程的最新更新 
git pull # 把你暂存的本地修改恢复回来 
git stash pop
```
如果这一步出现冲突，说明你的本地修改和远程的更新有内容重叠，解决完冲突后提交即可。

我们这里属于情况1，所以直接执行`git pull`就行：
![](Pasted%20image%2020260330220705.png)
![](Pasted%20image%2020260330220728.png)

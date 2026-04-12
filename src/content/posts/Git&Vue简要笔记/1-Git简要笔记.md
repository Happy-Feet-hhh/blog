---
title: Git简要笔记
published: 2026-03-30
description: "Git基础操作笔记：配置、初始化、暂存、提交与远程仓库"
tags: ["Git", "GitHub", "版本控制"]
category: Git&Vue
draft: false
---

# 配置全局名称和Email
```git
git config --global user.name "你的用户名"(Happy_Feet)
git config --global user.email "你的邮箱"(498949493@qq.com)
```
使用以下的命令可以查看global的配置
```bash
git config --global --list
```
![](Pasted%20image%2020260402210626.png)

# 初始化项目文件夹git
进入你的项目根文件夹，打入：
```
git init -b main
```
这会将我们的项目初始化为一个 **Git repo**，并创建了一个名为 `main` 的分支(Branch)，`git repo`是一个跟踪所有项目文件以及对它们更改的地方。
![](Pasted%20image%2020260330201201.png)

刚刚创建的main分支就像容器一样，它保留了我们项目所有的稳定版本的文件和更改。但现在它是空的，因为我们没有向里面添加任何文件，只有一个空的 `main branch`。
![](Pasted%20image%2020260330201216.png)

# 将文件添加到暂存区(Staging)
为了将我们的文件项目添加到repo中，需要先将文件添加到暂存区中，我们需要输入：
```git
git add .（当前文件夹下的所有内容，除了.gitignore的）
```
![](Pasted%20image%2020260330201700.png)
如上图所示，我们将所有文件添加到了暂存区，但是如何把它们添加到`main`分支呢？并且我们还需要追踪所有的变化内容，这个需要`git coommit` 将暂存区的文件提交到本地 Git 仓库，并且这个命令还支持 `-m` 参数 来添加评论注释。
```git
git commit -m "init"
```
![](Pasted%20image%2020260330202029.png)
## commit做了什么
`git commit`将我们在暂存区的文件，添加到了我们当前所在的分支中，当前即`main branch`。
![](Pasted%20image%2020260330202235.png)
同时，`branch`还保存了分支变化前后的状态，以便于我们回撤修改，当我们想要再次修改的时候，再次使用`git add`暂存文件，然后使用`git commit`来合并。
![](Pasted%20image%2020260402211015.png)
git管理的文件可以看下图：
![](Pasted%20image%2020260402211135.png)
# 连接到GitHub远程仓库(repo)
到目前为止我们都是在本地仓库里做的操作，想要将项目托管到GitHub，还需要一些操作。前提是我们需要创建一个GitHub账户(这一步省略)。然后通过SSH来验证身份，不然每一次变动都会被要求输入账密(非常麻烦)。

## **什么是 SSH 密钥？**

SSH 密钥是一种加密的认证方式，用于在不输入密码的情况下安全地访问远程服务器或 Git 仓库。它由一对密钥组成：

- **私钥**：保存在本地，切勿泄露。
- **公钥**：上传到 Git 服务器（如 GitHub），用于验证你的身份。

本地机器上的git ssh一版存储在`~`目录中，在Linux上是家目录，在Windows上是`C:\Users\你的用户名`
目录中，现在我们需要创建一个，在[GitHub的官方文档](C:\Users\49894)上可以搜索关键词`SSH`来查找到创建ssh的教程[具体网址](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)。
![](Pasted%20image%2020260330204741.png)
按照教程，我们输入命令：
```git
ssh-keygen -t rsa -b 4096 -C "498949493@qq.com"
```
1. `ssh-keygen`：生成、管理 SSH 密钥对的系统命令；
2. `-t rsa`：`-t` 指定密钥**加密类型**，`rsa` 是兼容性最强、最常用的非对称加密算法；
3. `-b 4096`：`-b` 指定密钥**长度（位数）**，4096 位是高安全规格（默认 2048 位），越长越难破解；
4. `-C "498949493@qq.com"`：`-C` 添加**注释**，仅用于标识密钥归属，方便区分不同密钥，无加密功能。

![](Pasted%20image%2020260330205030.png)
创建后，我们就可以在家目录找到：
![](Pasted%20image%2020260330205053.png)
![](Pasted%20image%2020260330205059.png)
带pub后缀的是公钥，不带的是私钥。然后我们在`powershell`终端里复制这个公钥的内容：
```powershell
Get-Content C:\Users\49894\.ssh\id_rsa.pub | Set-Clipboard
```
![](Pasted%20image%2020260330205741.png)

## **设置GitHub仓库**

### 创建GitHub的 SSH key

在GitHub网站中，找到个人设置。
![](Pasted%20image%2020260330205932.png)
然后在设置页面中找到`SSH`设置：
![](Pasted%20image%2020260330210047.png)
然后创建一个新的SSH key
![](Pasted%20image%2020260330210213.png)
随便起一个名字，然后粘贴刚才生成的ssh key：
![](Pasted%20image%2020260330210430.png)
点击`Add SSH Key`，即可完成。
![](Pasted%20image%2020260330210502.png)

### 创建新的Github仓库
完成后我们创建新的仓库，然后点击新建仓库：
![](Pasted%20image%2020260330210613.png)
![](Pasted%20image%2020260330210722.png)
完成之后会创建一个新的空仓库：
![](Pasted%20image%2020260330210801.png)
之后复制GitHub仓库的URL：
![](Pasted%20image%2020260330210817.png)

### git remote到GitHub远程仓库
使用`git remote`连接到刚才复制的url远程仓库：
```git
git remote add origin https://github.com/Happy-Feet-hhh/vuejs-masterclass-2024-Happy_Feet.git
```
#### 逐部分拆解

1. **git remote**：Git 中**管理远程仓库**的基础命令，用于配置本地代码与云端仓库的关联
2. **add**：子命令，作用是**新增**一个远程仓库关联
3. **origin**：给远程仓库起的**默认别名**（Git 约定俗成，代表主远程仓库，可自定义）
4. **后面的链接**：你的 **GitHub 远程仓库的唯一地址**

#### 整体含义

给**本地 Git 项目**，添加一个别名为 `origin` 的远程仓库，这个仓库指向你 GitHub 上的对应项目地址。

#### 核心作用

执行后，本地代码就绑定了云端仓库，后续可以用 `git push` 等命令，把本地代码推送到这个 GitHub 仓库中。

##### 总结

这是**本地项目关联 GitHub 远程仓库**的核心命令，建立本地与云端的连接。
![](Pasted%20image%2020260330211200.png)

### git push 推送到远程仓库
最终，使用`git push`将内容推送到远程仓库：
![](Pasted%20image%2020260330211357.png)
然后刷新GitHub仓库页面，发现已经更新：
![](Pasted%20image%2020260330211520.png)

```git
 git push -u origin main
```
这是**首次将本地主分支代码推送到远程仓库**的核心命令，拆分成 4 部分直白理解：

1. **git push**：基础命令，作用是**把本地代码提交到远程 Git 仓库**；
2. **-u**：`--set-upstream`的简写，**绑定本地分支和远程分支**（关键！第一次推送必须加），绑定后后续直接用`git push`就能推送，不用再写完整命令；
3. **origin**：远程仓库的**默认别名**（Git 自动生成，代表你托管代码的远程地址，比如 GitHub/GitLab 仓库）；
4. **main**：**分支名**（Git 新版默认主分支叫 main，老版是 master）。

# 后续更新
只需要
```bash
git add -A # 暂存所有的变更
git commit -m "message" # 将暂存区中的文件保存到本地repo中  (当前branch)
git push origin main  # 将main branch的repo推送到远程仓库中(origin是远程仓库的别名)
```

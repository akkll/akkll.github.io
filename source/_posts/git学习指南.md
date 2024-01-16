---
title: git学习指南
date: 2024-01-16 22:04:19
tags:
---

# git学习

## 1.安装配置

### 配置个人信息

```shell
git config --global user.name "用户名"
git config --global user.email 邮箱
```

## ssh连接

连接流程：

1.生成ssh密钥

```shell
ssh-keygen -t rsa -b 4096 -C "邮箱"
```

2.添加SSH密钥到SSH代理（可选）

```shell
ssh-add ~/.ssh/id_rsa
```

3.将SSH公钥添加到远程仓库

4.测试ssh连接

```shell
ssh -T git@github.com
```

## 2.仓库配置

### 1.建立远程仓库

```shell
git remote add origin(自选，就是当前远程仓库在本地的昵称) git@<你的用户名>/<仓库名>.git(这里去github复制)
```

### 2.删除远程仓库

```shell
git remote rm <remote-name>
```

### 3.查看远程仓库

```shell
# 查看所有远程仓库
git remote show
# 查看远程仓库的具体信息
git remote show <remote-name>
```

## 3.基础指令

{% asset_img 01.png %}

### 1.本地仓库初始化

```
git init <仓库路径，默认在本路径>
```

### 2.添加到暂存区

```shell
# 全部添加
git add .
# 逐个文件添加
git add <文件名>
```

### 3.提交到本地仓库

```
git commit -m "<提示信息>"
```

### 4.远程仓库

```shell
git push <仓库名> <本地分支名>:<远程分支名>
# 当<本地分支名>和<远程分支名>相同
git push <仓库名> <本地分支名>

git pull  拉取远程仓库所有分支更新并合并到本地分支。
git pull origin master 将远程master分支合并到当前本地master分支
git pull origin main:master 将远程min分支合并到当前本地master分支，冒号后面表示本地分支
```

### 5.查看状态

```
git status
```

### 6.克隆

```shell
git clone url  #克隆远程版本库
```

### 7.分支

```
# 查看全部分支
git branch
#查看所有远程的分支
git branch -r  
# 创建分支
git branch <branch_name>
# 删除分支
git branch -d <branch_name>
# 切换分支
# git checkout <branch_name>
# 合并
git merge master  在当前分支上合并master分支过来
```

### 8.日志

```
git log  查看提交历史和版本号，用于回退
git log --oneline 以精简模式显示查看提交历史
git log -p <file> 查看指定文件的提交历史
git blame <file> 一列表方式查看指定文件的提交历史
```

### 9.版本回退

```shell
git reset --hard + 版本号
```

### 10.撤销

```
#1.如果文件在工作目录中已被修改，但还没有被暂存（git add），这个命令会将文件还原到最后一次提交时的状态。
git restore --source=HEAD --worktree --staged -- <file>
#2.如果文件已经被暂存，但还没有提交，这个命令会取消暂存，将文件回退到上一次提交时的状态。
git reset HEAD file
git restore --source=HEAD --worktree --staged -- file
#3.如果文件已经被提交，直接版本回退
```



## 4.踩坑

### 1.git add时警告：warning: LF will be replaced by CRLF

格式化与多余的空白字符，特别是在跨平台情况下，有时候是一个令人发指的问题。由于编辑器的不同或者文件行尾的换行符在 Windows 下被替换了，一些细微的空格变化会不经意地混入提交，造成麻烦。虽然这是小问题，但它会极大地扰乱跨平台协作。
其实，这是因为在文本处理中，CR（CarriageReturn），LF（LineFeed），CR/LF是不同操作系统上使用的换行符。

#### 解决方案

```shell
#1.提交时转换为LF，检出时转换为CRLF
$ git config --global core.autocrlf true

#2.提交时转换为LF，检出时不转换
$ git config --global core.autocrlf input

#3.提交检出均不转换
$ git config --global core.autocrlf false
```

### 2.git推送错误

在push时爆了如下错误

```
! [rejected]        master -> main (non-fast-forward)
error: failed to push some refs to 
```

这个错误提示说明你的本地分支（master）和远程分支（main）存在不同的提交历史，并且远程分支的提交历史比本地的要新。

有可能是两个分支修改了同一个文件，解决方案如下：

#### 解决方案

1.使用 `git pull` 拉取远程分支的最新变更：

```shell
git pull origin main
```

2.查看哪些文件有冲突git status

3.打开冲突的文件，手动编辑文件，解决冲突，保留需要的修改。删除冲突标记（`<<<<<<<`, `=======`, `>>>>>>>`）并保留你需要的代码。

```
<<<<<<< HEAD 
// 你的修改 
======= 
// 远程分支的修改 
>>>>>>> branch-name
```

4.重新git add该文件

5.执行git commit -m ""

6.git push origin main

PS：实在不行就强制推过去：git push origin master:main --force🤭

### 3.ssh连不上远程仓库

报错信息

```
$ git remote show origin
ssh: connect to host github.com port 22: Connection timed out
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

解决方案：修改端口号

1.vim ~/.ssh.config

2.添加以下内容

```
Host github.com
User git
Hostname ssh.github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
Port 443

Host gitlab.com
Hostname altssh.gitlab.com
User git
Port 443
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa
```

3.检查

```
ssh -T git@github.com
```


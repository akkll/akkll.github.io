---
title: git学习
date: 2024-01-16 21:37:03
tags:git
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

```

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

### 2.

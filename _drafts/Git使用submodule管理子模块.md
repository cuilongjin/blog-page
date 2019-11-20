---
title: Git 使用 submodule 管理子模块
categories:
  - [工具]
date: 2019/10/20
---

子模块

git Submodule 是一个很好的多项目使用共同类库的工具，他允许类库项目做为 repository,子项目做为一个单独的 git 项目存在父项目中，子项目可以有自己的独立的 commit，push，pull。而父项目以 Submodule 的形式包含子项目，父项目可以指定子项目 header，父项目中会的提交信息包含 Submodule 的信息，再 clone 父项目的时候可以把 Submodule 初始化

使用 Git Submodule 解决下面几个问题：

A 项目，C 项目，B 库

1. 如何在 A 项目中使用 B 库
2. B 库在 A 项目中被修改了如何提交到 B 库远程库
3. C 项目如何获取 b 库最新的提交
4. clone A 项目时如何自动导入 B 库

### 添加

```bash
git submodule add <url> <path>
# url为子模块的路径，path为该子模块存储的目录路径
```

使用 git 命令向 A 项目中添加 Submodule B ：

```bash
git submodule add git@github.com:cuilongjin/B.git
```

使用 git status 命令可以看到

```
On branch master
Changes to be committed:
  new file:   .gitmodules
  new file:   B
```

.gitmodules 文件是一个配置文件,保存了项目 URL 和你拉取到的本地子目录

```
# .gitmodules
[submodule "B"]
path = B
url = git@github.com:cuilongjin/B.git
```

可以看到记录了子项目的目录和子项目的 git 地址信息。text 内容只保护子项目的 commit id，就能指定到对于的 git header 上。父项目的 git 并不会记录 Submodule 的文件变动，它是按照 commit git 指定 Submodule 的 git header。这两个文件都需要提交到父项目的 git 中。

git diff --cached 查看修改内容可以看到增加了子模块，并且新文件下为子模块的提交 hash 摘要

git commit 提交即完成子模块的添加

### 修改 Submodule

需要确认有对 Submodule B 的 commit 权限

进入 Submodule B 目录里面，提交 Submodule 的更改内容：

git add
git commit -m'submodule A'

然后 push 到远程服务器: git push

然后再回到父目录,提交 Submodule B 在父项目中的变动：

```
git status
on branch master
  modified: B (new commits)
```

可以看到 B 中已经变更为 Submodule 最新的 commit id:

```
Subproject commit *********
```

把 Submodule 的变动信息推送到父项目的远程服务器

```
git commit -m'update submodule B'
git push
```

这样就把子模块的变更信息以及子模块的变更信息提交到远程服务器了，从远程服务器上更新下来的内容就是最新提交的内容了

### 使用

克隆项目后，默认子模块目录下无任何内容。需要在项目根目录执行如下命令完成子模块的下载：

```sh
git submodule init
git submodule update
```

### 在父项目中更新 Submodule

在父项目的目录下直接运行

```
git submodule foreach git pull
```

在 Submodule B 的目录下面更新

```
cd B
git pull
```

### 克隆一个带子模块的项目

1. 采用递归参数 --recursive

```
git clone git@github.com:cuilongiin/ --recursive
```

2. 第二种方法先 clone 父项目，再初始化 Submodule

```
git clone git@github.com:cuilongjin/**
cd B
git submodule init // 初始化你的本地配置文件
git submodule update //
```

### 删除 Submodule

```
git rm [submodule name]
```

将一个 git 仓库里的一部分文件转出作为一个独立的仓库并保留提交记录 commit log

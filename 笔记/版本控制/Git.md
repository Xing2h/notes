[TOC]



### 1、安装

```bash
git config --global user.name "名称"
git config --global user.email "邮箱"
```

### 2、创建版本库

在想要创建版本库的位置，创建新的空目录（建一个新文件夹）

```bash
mkdir 目录名（文件夹名） // 创建一个新的目录（文件夹）
cd 要找的目录 // 找到创建的目录
pwd // 显示当前目录
git init // 初始化版本库
```
创建的文件夹为工作区

工作区中的 .git 文件夹是 git 的版本库

### 3、版本控制

#### 1、版本回退

```bash
git add file_name // 把文件添加到暂存区
git commit -m "本次提交说明" // 把文件由暂存区提交到当前分支
git status // 查看当前仓库的状态
git diff 文件名 // 查看文件的不同/差异
git log // 查看提交日志 可以确定回退到哪个版本
git log --pretty=oneline // 每条记录展示在一行里
git reset --hard HEAD^ // 把当前版本回退到上一个版本  HEAD指向的就是当前版本
git reset --hard HEAD^^ // 把当前版本回退到上两个版本
git reset --hard HEAD~100 // 把当前版本回退到上100个版本
git reset --hard (commit id的前几位) // 回到指定的某个版本
git reflog // 查看历史命令 确定回到未来的哪个版本
git clone <仓库地址>//克隆仓库到本地
git push origin master

```

####  2、管理修改

git 管理的是对文件的修改

git add 命令是把文件的修改提交到暂存区

git commit 命令是把修改提交到仓库

如果对文件进行一次修改，进行 git add ，然后对文件进行第二次修改，这个时候 git commit ，提交的文件还是第一次修改后的内容

3、git push出错

```bash
//取消全局代理
$ git config --global --unset https.proxy
//本地删除远程仓库
git remote rm tee

```

